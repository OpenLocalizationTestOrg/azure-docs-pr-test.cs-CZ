---
title: "Pokročilé zásady Azure API Management | Microsoft Docs"
description: "Další informace o pokročilé zásady, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8a13348b-7856-428f-8e35-9e4273d94323
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 0c65ac74316421a0258f01143baa25ffecb5be3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="api-management-advanced-policies"></a><span data-ttu-id="dda91-103">Pokročilé zásady API Management</span><span class="sxs-lookup"><span data-stu-id="dda91-103">API Management advanced policies</span></span>
<span data-ttu-id="dda91-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="dda91-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="dda91-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="dda91-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="dda91-106"><a name="AdvancedPolicies"></a>Rozšířené zásady</span><span class="sxs-lookup"><span data-stu-id="dda91-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="dda91-107">[Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad na základě výsledků vyhodnocení logická hodnota [výrazy](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="dda91-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on the results of the evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="dda91-108">[Předat dál žádost](#ForwardRequest) -předá požadavek back-end službu.</span><span class="sxs-lookup"><span data-stu-id="dda91-108">[Forward request](#ForwardRequest) - Forwards the request to the backend service.</span></span>

-   <span data-ttu-id="dda91-109">[Omezit souběžnosti](#LimitConcurrency) -brání uzavřena zásady z provádění více než určitý počet požadavků současně.</span><span class="sxs-lookup"><span data-stu-id="dda91-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than the specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="dda91-110">[Protokol do centra událostí](#log-to-eventhub) -odešle zprávy v zadaném formátu do centra událostí definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="dda91-110">[Log to Event Hub](#log-to-eventhub) - Sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="dda91-111">[Model odpovědi](#mock-response) -přerušení způsobených kanálu provádění a vrátí mocked odpověď přímo na volajícího.</span><span class="sxs-lookup"><span data-stu-id="dda91-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly to the caller.</span></span>
  
-   <span data-ttu-id="dda91-112">[Opakujte](#Retry) -opakování provádění příkazů závorkách zásad, pokud a dokud nebude splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="dda91-112">[Retry](#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="dda91-113">Spuštění se opakovaly zadaným časovým intervalům a až zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-113">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>  
  
-   <span data-ttu-id="dda91-114">[Vrátí odpověď](#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí zadanou odpověď přímo na volajícího.</span><span class="sxs-lookup"><span data-stu-id="dda91-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span> 
  
-   <span data-ttu-id="dda91-115">[Odeslání žádosti o jednorázové jednoduché](#SendOneWayRequest) -odešle požadavek na zadanou adresu URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="dda91-115">[Send one way request](#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="dda91-116">[Odeslání požadavku](#SendRequest) -odešle požadavek na zadanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dda91-116">[Send request](#SendRequest) - Sends a request to the specified URL.</span></span>  

-   <span data-ttu-id="dda91-117">[Nastavení proxy serveru HTTP](#SetHttpProxy) -umožňuje trasy předané požadavky prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="dda91-117">[Set HTTP proxy](#SetHttpProxy) - Allows you to route forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="dda91-118">[Nastaví metodu požadavku](#SetRequestMethod) -vám umožní změnit metodu protokolu HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="dda91-118">[Set request method](#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="dda91-119">[Nastavit stavový kód](#SetStatus) -změní stavový kód protokolu HTTP se zadanou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="dda91-119">[Set status code](#SetStatus) - Changes the HTTP status code to the specified value.</span></span>  
  
-   <span data-ttu-id="dda91-120">[Nastavená proměnná](api-management-advanced-policies.md#set-variable) -potrvají hodnotu v pojmenovaná [kontextu](api-management-policy-expressions.md#ContextVariables) proměnná pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="dda91-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="dda91-121">[Trasování](#Trace) -přidá řetězec do [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="dda91-121">[Trace](#Trace) - Adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="dda91-122">[Počkejte](#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady k dokončení než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="dda91-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies to complete before proceeding.</span></span>  
  
##  <span data-ttu-id="dda91-123"><a name="choose"></a>Tok řízení</span><span class="sxs-lookup"><span data-stu-id="dda91-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="dda91-124">`choose` Zásada závorkách zásady vytvořit na základě výsledku vyhodnocení logické výrazy, podobně jako if pak else nebo přepínač příkazy v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="dda91-124">The `choose` policy applies enclosed policy statements based on the outcome of evaluation of Boolean expressions, similar to an if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="dda91-125"><a name="ChoosePolicyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 <span data-ttu-id="dda91-126">Zásada řízení toku musí obsahovat alespoň jeden `<when/>` elementu.</span><span class="sxs-lookup"><span data-stu-id="dda91-126">The control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="dda91-127">`<otherwise/>` Prvek je volitelný.</span><span class="sxs-lookup"><span data-stu-id="dda91-127">The `<otherwise/>` element is optional.</span></span> <span data-ttu-id="dda91-128">Podmínky v `<when/>` elementy jsou vyhodnocovány v pořadí podle jejich vzhled v rámci zásady.</span><span class="sxs-lookup"><span data-stu-id="dda91-128">Conditions in `<when/>` elements are evaluated in order of their appearance within the policy.</span></span> <span data-ttu-id="dda91-129">Příkazy zásad v rámci první uzavřené `<when/>` element s podmínku atributu rovná `true` se použijí.</span><span class="sxs-lookup"><span data-stu-id="dda91-129">Policy statement(s) enclosed within the first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="dda91-130">Zásady v rámci uzavřené `<otherwise/>` elementu, pokud existuje, bude použito v případě všech z `<when/>` element podmínku atributy jsou `false`.</span><span class="sxs-lookup"><span data-stu-id="dda91-130">Policies enclosed within the `<otherwise/>` element, if present, will be applied if all of the `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="dda91-131">Příklady</span><span class="sxs-lookup"><span data-stu-id="dda91-131">Examples</span></span>  
  
####  <span data-ttu-id="dda91-132"><a name="ChooseExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="dda91-133">Následující příklad ukazuje [nastavená proměnná](api-management-advanced-policies.md#set-variable) zásady a dvě zásady řízení toku.</span><span class="sxs-lookup"><span data-stu-id="dda91-133">The following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="dda91-134">Nastavit proměnnou zásady je v části příchozí a vytvoří `isMobile` Boolean [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena na hodnotu true, pokud `User-Agent` žádosti záhlaví obsahuje text `iPad` nebo `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="dda91-134">The set variable policy is in the inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="dda91-135">První zásada toku řízení je taky v části příchozí a podmíněně použije jednu ze dvou [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásady v závislosti na hodnotě `isMobile` kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="dda91-135">The first control flow policy is also in the inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on the value of the `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="dda91-136">Druhá zásada toku řízení je v části odchozí a podmíněně se vztahuje [XML převést na JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) zásady při `isMobile` je nastaven na `true`.</span><span class="sxs-lookup"><span data-stu-id="dda91-136">The second control flow policy is in the outbound section and conditionally applies the [Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set to `true`.</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a><span data-ttu-id="dda91-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-137">Example</span></span>  
 <span data-ttu-id="dda91-138">Tento příklad ukazuje, jak provést filtrování obsahu odebráním datové prvky. z odpovědi přijal od služby back-end při použití `Starter` produktu.</span><span class="sxs-lookup"><span data-stu-id="dda91-138">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="dda91-139">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 34:30.</span><span class="sxs-lookup"><span data-stu-id="dda91-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="dda91-140">Spuštění na 31:50 zobrazíte přehled [rozhraní tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="dda91-140">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="dda91-141">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-141">Elements</span></span>  
  
|<span data-ttu-id="dda91-142">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-142">Element</span></span>|<span data-ttu-id="dda91-143">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-143">Description</span></span>|<span data-ttu-id="dda91-144">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-145">Zvolte</span><span class="sxs-lookup"><span data-stu-id="dda91-145">choose</span></span>|<span data-ttu-id="dda91-146">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-146">Root element.</span></span>|<span data-ttu-id="dda91-147">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-147">Yes</span></span>|  
|<span data-ttu-id="dda91-148">Kdy</span><span class="sxs-lookup"><span data-stu-id="dda91-148">when</span></span>|<span data-ttu-id="dda91-149">Podmínku, která má použít pro `if` nebo `ifelse` části `choose` zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-149">The condition to use for the `if` or `ifelse` parts of the `choose` policy.</span></span> <span data-ttu-id="dda91-150">Pokud `choose` zásad obsahuje více `when` oddíly, jsou vyhodnocovány postupně.</span><span class="sxs-lookup"><span data-stu-id="dda91-150">If the `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="dda91-151">Jednou `condition` z když se vyhodnocuje element `true`, žádné další `when` vyhodnocení podmínek.</span><span class="sxs-lookup"><span data-stu-id="dda91-151">Once the `condition` of a when element evaluates to `true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="dda91-152">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-152">Yes</span></span>|  
|<span data-ttu-id="dda91-153">v opačném případě</span><span class="sxs-lookup"><span data-stu-id="dda91-153">otherwise</span></span>|<span data-ttu-id="dda91-154">Obsahuje fragmentu zásad, který se má použít, pokud žádná z `when` vyhodnocení podmínky `true`.</span><span class="sxs-lookup"><span data-stu-id="dda91-154">Contains the policy snippet to be used if none of the `when` conditions evaluate to `true`.</span></span>|<span data-ttu-id="dda91-155">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-156">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-156">Attributes</span></span>  
  
|<span data-ttu-id="dda91-157">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-157">Attribute</span></span>|<span data-ttu-id="dda91-158">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-158">Description</span></span>|<span data-ttu-id="dda91-159">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="dda91-160">podmínka = "logický výraz &#124; Logická hodnota konstanta"</span><span class="sxs-lookup"><span data-stu-id="dda91-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="dda91-161">Logický výraz nebo konstantu, která vyhodnotí při obsahující `when` zásad vyhodnotí.</span><span class="sxs-lookup"><span data-stu-id="dda91-161">The Boolean expression or constant to evaluated when the containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="dda91-162">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-162">Yes</span></span>|  
  
###  <span data-ttu-id="dda91-163"><a name="ChooseUsage"></a>Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="dda91-164">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-164">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-165">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-166">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-167"><a name="ForwardRequest"></a>Předat dál požadavku</span><span class="sxs-lookup"><span data-stu-id="dda91-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="dda91-168">`forward-request` Zásada předá příchozí požadavek na back-end službu zadanou v žádosti [kontextu](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="dda91-168">The `forward-request` policy forwards the incoming request to the backend service specified in the request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="dda91-169">Adresa URL back-end služby je zadaný v rozhraní API [nastavení](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) a můžete změnit pomocí [nastavení back-end služby](api-management-transformation-policies.md) zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-169">The backend service URL is specified in the API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using the [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="dda91-170">Odebrání výsledků této zásady v požadavku není předávaných do back-end služby a zásady v části odchozí vyhodnocují hned po úspěšném dokončení zásad v části příchozí.</span><span class="sxs-lookup"><span data-stu-id="dda91-170">Removing this policy results in the request not being forwarded to the backend service and the policies in the outbound section are evaluated immediately upon the successful completion of the policies in the inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-171">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="dda91-172">Příklady</span><span class="sxs-lookup"><span data-stu-id="dda91-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="dda91-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-173">Example</span></span>  
 <span data-ttu-id="dda91-174">Tyto zásady úrovně rozhraní API předává všechny požadavky na back-end služby s časový limit na 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="dda91-174">The following API level policy forwards all requests to the backend service with a timeout interval of 60 seconds.</span></span>  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="dda91-175">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-175">Example</span></span>  
 <span data-ttu-id="dda91-176">Tato zásada na úrovni operace používá `base` element dědí zásady back-end z nadřazeného oboru úrovně rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dda91-176">This operation level policy uses the `base` element to inherit the backend policy from the parent API level scope.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="dda91-177">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-177">Example</span></span>  
 <span data-ttu-id="dda91-178">Tato zásada na úrovni operaci explicitně předává všechny požadavky na back-end službu s časovým limitem 120 a nedědí nadřazené úrovně back-end zásada rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dda91-178">This operation level policy explicitly forwards all requests to the backend service with a timeout of 120 and does not inherit the parent API level backend policy.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="dda91-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-179">Example</span></span>  
 <span data-ttu-id="dda91-180">Tato zásada na úrovni operaci nepředává požadavky na back-end službu.</span><span class="sxs-lookup"><span data-stu-id="dda91-180">This operation level policy does not forward requests to the backend service.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-181">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-181">Elements</span></span>  
  
|<span data-ttu-id="dda91-182">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-182">Element</span></span>|<span data-ttu-id="dda91-183">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-183">Description</span></span>|<span data-ttu-id="dda91-184">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-185">předání požadavku</span><span class="sxs-lookup"><span data-stu-id="dda91-185">forward-request</span></span>|<span data-ttu-id="dda91-186">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-186">Root element.</span></span>|<span data-ttu-id="dda91-187">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-188">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-188">Attributes</span></span>  
  
|<span data-ttu-id="dda91-189">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-189">Attribute</span></span>|<span data-ttu-id="dda91-190">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-190">Description</span></span>|<span data-ttu-id="dda91-191">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-191">Required</span></span>|<span data-ttu-id="dda91-192">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-193">časový limit = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="dda91-193">timeout="integer"</span></span>|<span data-ttu-id="dda91-194">Interval vypršení časového limitu v sekundách, než volání službě back-end se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="dda91-194">The timeout interval in seconds before the call to the backend service fails.</span></span>|<span data-ttu-id="dda91-195">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-195">No</span></span>|<span data-ttu-id="dda91-196">Bez časového limitu</span><span class="sxs-lookup"><span data-stu-id="dda91-196">No timeout</span></span>|  
|<span data-ttu-id="dda91-197">postupujte podle přesměrování = "true &#124; false"</span><span class="sxs-lookup"><span data-stu-id="dda91-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="dda91-198">Určuje, zda jsou přesměrování z back-end službu následuje bránu nebo vrácen volajícímu.</span><span class="sxs-lookup"><span data-stu-id="dda91-198">Specifies whether redirects from the backend service are followed by the gateway or returned to the caller.</span></span>|<span data-ttu-id="dda91-199">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-199">No</span></span>|<span data-ttu-id="dda91-200">False</span><span class="sxs-lookup"><span data-stu-id="dda91-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-201">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-201">Usage</span></span>  
 <span data-ttu-id="dda91-202">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-202">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-203">**Části zásady:** back-end</span><span class="sxs-lookup"><span data-stu-id="dda91-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="dda91-204">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-205"><a name="LimitConcurrency"></a>Limit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="dda91-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="dda91-206">`limit-concurrency` Zásady zabrání závorkách zásady provádění více než určitý počet požadavků v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="dda91-206">The `limit-concurrency` policy prevents enclosed policies from executing by more than the specified number of requests at a given time.</span></span> <span data-ttu-id="dda91-207">Při překračující prahovou hodnotu, se přidají nové žádosti o do fronty, dokud nedosáhnete délka maximální fronty.</span><span class="sxs-lookup"><span data-stu-id="dda91-207">Upon exceeding the threshold, new requests are added to a queue, until the maximum queue length is achieved.</span></span> <span data-ttu-id="dda91-208">Po vyčerpání fronty nové požadavky selže okamžitě.</span><span class="sxs-lookup"><span data-stu-id="dda91-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="dda91-209"><a name="LimitConcurrencyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="dda91-210">Příklady</span><span class="sxs-lookup"><span data-stu-id="dda91-210">Examples</span></span>  
  
####  <span data-ttu-id="dda91-211"><a name="ChooseExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="dda91-212">Následující příklad ukazuje, jak omezit počet požadavků, které jsou předávány back-end na základě hodnoty proměnné kontextu.</span><span class="sxs-lookup"><span data-stu-id="dda91-212">The following example demonstrates how to limit number of requests forwarded to a backend based on the value of a context variable.</span></span>
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3" timeout="60">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a><span data-ttu-id="dda91-213">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-213">Elements</span></span>  
  
|<span data-ttu-id="dda91-214">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-214">Element</span></span>|<span data-ttu-id="dda91-215">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-215">Description</span></span>|<span data-ttu-id="dda91-216">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="dda91-217">limit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="dda91-217">limit-concurrency</span></span>|<span data-ttu-id="dda91-218">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-218">Root element.</span></span>|<span data-ttu-id="dda91-219">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-220">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-220">Attributes</span></span>  
  
|<span data-ttu-id="dda91-221">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-221">Attribute</span></span>|<span data-ttu-id="dda91-222">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-222">Description</span></span>|<span data-ttu-id="dda91-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-223">Required</span></span>|<span data-ttu-id="dda91-224">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="dda91-225">key</span><span class="sxs-lookup"><span data-stu-id="dda91-225">key</span></span>|<span data-ttu-id="dda91-226">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="dda91-226">A string.</span></span> <span data-ttu-id="dda91-227">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="dda91-227">Expression allowed.</span></span> <span data-ttu-id="dda91-228">Určuje obor souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-228">Specifies the concurrency scope.</span></span> <span data-ttu-id="dda91-229">Může být sdílen více zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="dda91-230">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-230">Yes</span></span>|<span data-ttu-id="dda91-231">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-231">N/A</span></span>|  
|<span data-ttu-id="dda91-232">maximální počet</span><span class="sxs-lookup"><span data-stu-id="dda91-232">max-count</span></span>|<span data-ttu-id="dda91-233">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dda91-233">An integer.</span></span> <span data-ttu-id="dda91-234">Určuje maximální počet požadavků, které jsou k zadání zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-234">Specifies a maximum number of requests that are allowed to enter the policy.</span></span>|<span data-ttu-id="dda91-235">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-235">Yes</span></span>|<span data-ttu-id="dda91-236">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-236">N/A</span></span>|  
|<span data-ttu-id="dda91-237">Časový limit</span><span class="sxs-lookup"><span data-stu-id="dda91-237">timeout</span></span>|<span data-ttu-id="dda91-238">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dda91-238">An integer.</span></span> <span data-ttu-id="dda91-239">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="dda91-239">Expression allowed.</span></span> <span data-ttu-id="dda91-240">Určuje počet sekund žádost měli počkat, zadejte obor než selže s "403 příliš mnoho požadavků"</span><span class="sxs-lookup"><span data-stu-id="dda91-240">Specifies the number of seconds a request should wait to enter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="dda91-241">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-241">No</span></span>|<span data-ttu-id="dda91-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="dda91-242">Infinity</span></span>|  
|<span data-ttu-id="dda91-243">Maximální délka fronty</span><span class="sxs-lookup"><span data-stu-id="dda91-243">max-queue-length</span></span>|<span data-ttu-id="dda91-244">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="dda91-244">An integer.</span></span> <span data-ttu-id="dda91-245">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="dda91-245">Expression allowed.</span></span> <span data-ttu-id="dda91-246">Určuje maximální fronty.</span><span class="sxs-lookup"><span data-stu-id="dda91-246">Specifies the maximum queue length.</span></span> <span data-ttu-id="dda91-247">Příchozí žádosti o pokusu zadejte tuto zásadu bude ukončena s "403 příliš mnoho požadavků" okamžitě po vyčerpání fronty.</span><span class="sxs-lookup"><span data-stu-id="dda91-247">Incoming requests trying to enter this policy will be terminated with “403 Too Many Requests” immediately when the queue is exhausted.</span></span>|<span data-ttu-id="dda91-248">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-248">No</span></span>|<span data-ttu-id="dda91-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="dda91-249">Infinity</span></span>|  
  
###  <span data-ttu-id="dda91-250"><a name="ChooseUsage"></a>Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="dda91-251">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-251">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-252">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-253">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="dda91-254"><a name="log-to-eventhub"></a>Protokol do centra událostí</span><span class="sxs-lookup"><span data-stu-id="dda91-254"><a name="log-to-eventhub"></a> Log to Event Hub</span></span>  
 <span data-ttu-id="dda91-255">`log-to-eventhub` Zásad odešle zprávy v zadaném formátu do centra událostí definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="dda91-255">The `log-to-eventhub` policy sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="dda91-256">Jak již název napovídá, zásady se používá pro uložení vybraného požadavku nebo odpovědi kontextové informace pro analýzu online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="dda91-256">As its name implies, the policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="dda91-257">Podrobné informace o konfiguraci centra událostí a protokolování událostí najdete v tématu [protokolování událostí správy rozhraní API pro Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="dda91-257">For a step-by-step guide on configuring an event hub and logging events, see [How to log API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-258">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-259">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-259">Example</span></span>  
 <span data-ttu-id="dda91-260">Libovolný řetězec slouží jako hodnota má být přihlášen Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="dda91-260">Any string can be used as the value to be logged in Event Hubs.</span></span> <span data-ttu-id="dda91-261">V tomto příkladu datum a čas, název služby pro nasazení, id žádosti, ip adresu a název operace pro všechny příchozí volání se protokolují do centra událostí protokolovacího nástroje zaregistrována `contoso-logger` id.</span><span class="sxs-lookup"><span data-stu-id="dda91-261">In this example the date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged to the event hub Logger registered with the `contoso-logger` id.</span></span>  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-262">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-262">Elements</span></span>  
  
|<span data-ttu-id="dda91-263">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-263">Element</span></span>|<span data-ttu-id="dda91-264">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-264">Description</span></span>|<span data-ttu-id="dda91-265">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-266">protokol eventhub</span><span class="sxs-lookup"><span data-stu-id="dda91-266">log-to-eventhub</span></span>|<span data-ttu-id="dda91-267">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-267">Root element.</span></span> <span data-ttu-id="dda91-268">Hodnota tohoto elementu je řetězec k přihlášení do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="dda91-268">The value of this element is the string to log to your event hub.</span></span>|<span data-ttu-id="dda91-269">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-270">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-270">Attributes</span></span>  
  
|<span data-ttu-id="dda91-271">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-271">Attribute</span></span>|<span data-ttu-id="dda91-272">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-272">Description</span></span>|<span data-ttu-id="dda91-273">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="dda91-274">id protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="dda91-274">logger-id</span></span>|<span data-ttu-id="dda91-275">Id protokolovacího nástroje registrován u služby API Management.</span><span class="sxs-lookup"><span data-stu-id="dda91-275">The id of the Logger registered with your API Management service.</span></span>|<span data-ttu-id="dda91-276">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-276">Yes</span></span>|  
|<span data-ttu-id="dda91-277">id oddílu</span><span class="sxs-lookup"><span data-stu-id="dda91-277">partition-id</span></span>|<span data-ttu-id="dda91-278">Určuje index oddílu, které jsou odesílány zprávy.</span><span class="sxs-lookup"><span data-stu-id="dda91-278">Specifies the index of the partition where messages are sent.</span></span>|<span data-ttu-id="dda91-279">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="dda91-279">Optional.</span></span> <span data-ttu-id="dda91-280">Tento atribut nemusí být použit, pokud `partition-key` se používá.</span><span class="sxs-lookup"><span data-stu-id="dda91-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="dda91-281">klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="dda91-281">partition-key</span></span>|<span data-ttu-id="dda91-282">Určuje hodnotu sloužící pro přiřazení k oddílu, odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="dda91-282">Specifies the value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="dda91-283">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="dda91-283">Optional.</span></span> <span data-ttu-id="dda91-284">Tento atribut nemusí být použit, pokud `partition-id` se používá.</span><span class="sxs-lookup"><span data-stu-id="dda91-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-285">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-285">Usage</span></span>  
 <span data-ttu-id="dda91-286">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-286">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-287">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-288">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="dda91-289"><a name="mock-response"></a>Imitované odpovědi</span><span class="sxs-lookup"><span data-stu-id="dda91-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="dda91-290">`mock-response`, Jako název znamená, že se používá model rozhraní API a operace.</span><span class="sxs-lookup"><span data-stu-id="dda91-290">The `mock-response`, as the name implies, is used to mock APIs and operations.</span></span> <span data-ttu-id="dda91-291">To zruší provádění běžný kanál a vrátí mocked odpověď na volajícího.</span><span class="sxs-lookup"><span data-stu-id="dda91-291">It aborts normal pipeline execution and returns a mocked response to the caller.</span></span> <span data-ttu-id="dda91-292">Zásady se vždy pokusí vrátit odpovědí nejvyšší přesnosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-292">The policy always tries to return responses of highest fidelity.</span></span> <span data-ttu-id="dda91-293">Dává přednost obsahu příklady odpovědi, vždy, když je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="dda91-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="dda91-294">Generuje ukázka odpovědí z schémata, když jsou k dispozici schémata a příklady nejsou.</span><span class="sxs-lookup"><span data-stu-id="dda91-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="dda91-295">Pokud ani příklady nebo schémata jsou nalezena, vrátí se odpovědi s žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="dda91-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-296">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="dda91-297">Příklady</span><span class="sxs-lookup"><span data-stu-id="dda91-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-298">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-298">Elements</span></span>  
  
|<span data-ttu-id="dda91-299">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-299">Element</span></span>|<span data-ttu-id="dda91-300">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-300">Description</span></span>|<span data-ttu-id="dda91-301">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-302">model odpovědi</span><span class="sxs-lookup"><span data-stu-id="dda91-302">mock-response</span></span>|<span data-ttu-id="dda91-303">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-303">Root element.</span></span>|<span data-ttu-id="dda91-304">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-305">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-305">Attributes</span></span>  
  
|<span data-ttu-id="dda91-306">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-306">Attribute</span></span>|<span data-ttu-id="dda91-307">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-307">Description</span></span>|<span data-ttu-id="dda91-308">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-308">Required</span></span>|<span data-ttu-id="dda91-309">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="dda91-310">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="dda91-310">status-code</span></span>|<span data-ttu-id="dda91-311">Určuje stavový kód odpovědi a slouží k výběru odpovídající příklad nebo schéma.</span><span class="sxs-lookup"><span data-stu-id="dda91-311">Specifies response status code and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="dda91-312">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-312">No</span></span>|<span data-ttu-id="dda91-313">200</span><span class="sxs-lookup"><span data-stu-id="dda91-313">200</span></span>|  
|<span data-ttu-id="dda91-314">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="dda91-314">content-type</span></span>|<span data-ttu-id="dda91-315">Určuje `Content-Type` hodnota hlavičky odpovědi a slouží k výběru odpovídající příklad nebo schéma.</span><span class="sxs-lookup"><span data-stu-id="dda91-315">Specifies `Content-Type` response header value and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="dda91-316">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-316">No</span></span>|<span data-ttu-id="dda91-317">Žádný</span><span class="sxs-lookup"><span data-stu-id="dda91-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-318">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-318">Usage</span></span>  
 <span data-ttu-id="dda91-319">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-319">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-320">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="dda91-321">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="dda91-322"><a name="Retry"></a>Opakování</span><span class="sxs-lookup"><span data-stu-id="dda91-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="dda91-323">`retry` Zásady provede jeho podřízených zásady jednou a pak opakuje jejich spuštění, dokud opakovaném `condition` stane `false` nebo opakujte `count` je vyčerpání.</span><span class="sxs-lookup"><span data-stu-id="dda91-323">The             `retry` policy executes its child policies once and then retries their execution until the retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-324">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-324">Policy statement</span></span>  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-325">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-325">Example</span></span>  
 <span data-ttu-id="dda91-326">V následujícím příkladu žádosti forewarding opakována až desetkrát pomocí exponenciální opakujte algoritmus.</span><span class="sxs-lookup"><span data-stu-id="dda91-326">In the following example request forewarding is retried up to ten times using exponential retry algorithm.</span></span> <span data-ttu-id="dda91-327">Vzhledem k tomu `first-fast-retry` není nastaven na hodnotu false, všechny pokusy o opakování se vztahují algoritmus exponsntial opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-327">Since                    `first-fast-retry` is set to false, all retry attempts are subject to the exponsntial retry algorithm.</span></span>  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-328">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-328">Elements</span></span>  
  
|<span data-ttu-id="dda91-329">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-329">Element</span></span>|<span data-ttu-id="dda91-330">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-330">Description</span></span>|<span data-ttu-id="dda91-331">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-332">Opakování</span><span class="sxs-lookup"><span data-stu-id="dda91-332">retry</span></span>|<span data-ttu-id="dda91-333">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-333">Root element.</span></span> <span data-ttu-id="dda91-334">Může obsahovat další zásady jako jeho podřízených elementů.</span><span class="sxs-lookup"><span data-stu-id="dda91-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="dda91-335">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-336">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-336">Attributes</span></span>  
  
|<span data-ttu-id="dda91-337">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-337">Attribute</span></span>|<span data-ttu-id="dda91-338">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-338">Description</span></span>|<span data-ttu-id="dda91-339">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-339">Required</span></span>|<span data-ttu-id="dda91-340">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-341">Podmínka</span><span class="sxs-lookup"><span data-stu-id="dda91-341">condition</span></span>|<span data-ttu-id="dda91-342">Logická hodnota literál nebo [výraz](api-management-policy-expressions.md) zadání, pokud by se měla zastavit opakovaných pokusů (`false`) nebo pokračování (`true`).</span><span class="sxs-lookup"><span data-stu-id="dda91-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="dda91-343">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-343">Yes</span></span>|<span data-ttu-id="dda91-344">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-344">N/A</span></span>|  
|<span data-ttu-id="dda91-345">Počet</span><span class="sxs-lookup"><span data-stu-id="dda91-345">count</span></span>|<span data-ttu-id="dda91-346">Kladné číslo určující maximální počet opakování pokusu.</span><span class="sxs-lookup"><span data-stu-id="dda91-346">A positive number specifying the maximum number of retries to attempt.</span></span>|<span data-ttu-id="dda91-347">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-347">Yes</span></span>|<span data-ttu-id="dda91-348">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-348">N/A</span></span>|  
|<span data-ttu-id="dda91-349">Interval</span><span class="sxs-lookup"><span data-stu-id="dda91-349">interval</span></span>|<span data-ttu-id="dda91-350">Pokusí se v sekundách zadání intervalu čekání mezi opakovaném kladné číslo.</span><span class="sxs-lookup"><span data-stu-id="dda91-350">A positive number in seconds specifying the wait interval between the retry attempts.</span></span>|<span data-ttu-id="dda91-351">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-351">Yes</span></span>|<span data-ttu-id="dda91-352">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-352">N/A</span></span>|  
|<span data-ttu-id="dda91-353">maximální interval</span><span class="sxs-lookup"><span data-stu-id="dda91-353">max-interval</span></span>|<span data-ttu-id="dda91-354">Kladné číslo v sekundách určení maximální Počkejte, než interval mezi pokusy o opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-354">A positive number in seconds specifying the maximum wait interval between the retry attempts.</span></span> <span data-ttu-id="dda91-355">Slouží k implementaci algoritmu exponenciální opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-355">It is used to implement an exponential retry algorithm.</span></span>|<span data-ttu-id="dda91-356">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-356">No</span></span>|<span data-ttu-id="dda91-357">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-357">N/A</span></span>|  
|<span data-ttu-id="dda91-358">rozdílů</span><span class="sxs-lookup"><span data-stu-id="dda91-358">delta</span></span>|<span data-ttu-id="dda91-359">Kladné číslo v sekundách zadání přírůstek intervalu čekání.</span><span class="sxs-lookup"><span data-stu-id="dda91-359">A positive number in seconds specifying the wait interval increment.</span></span> <span data-ttu-id="dda91-360">Slouží k implementaci algoritmy lineární a exponenciální opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-360">It is used to implement the linear and exponential retry algorithms.</span></span>|<span data-ttu-id="dda91-361">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-361">No</span></span>|<span data-ttu-id="dda91-362">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-362">N/A</span></span>|  
|<span data-ttu-id="dda91-363">Zkuste zopakovat první fast</span><span class="sxs-lookup"><span data-stu-id="dda91-363">first-fast-retry</span></span>|<span data-ttu-id="dda91-364">Pokud nastavena na `true` , první pokus o opakování provádí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="dda91-364">If set to                                    `true` , the first retry attempt is performed immediately.</span></span>|<span data-ttu-id="dda91-365">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="dda91-366">Když pouze `interval` není zadaný, **pevné** jsou prováděny interval opakování.</span><span class="sxs-lookup"><span data-stu-id="dda91-366">When only the `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="dda91-367">Když pouze `interval` a `delta` nejsou zadány, **lineární** se použije algoritmus interval opakování, kde se počítá doba čekání mezi opakovanými pokusy podle následující vzorec - `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="dda91-367">When only the `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according the following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="dda91-368">Když `interval`, `max-interval` a `delta` jsou nastaveny, **exponenciální** interval opakování algoritmus se použije, kde je doba čekání mezi jednotlivými pokusy o odeslání zvětšování exponenciálně od hodnoty `interval` k Hodnota `max-interval` podle následující forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="dda91-368">When the `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where the wait time between the retries is growing exponentially from the value of `interval` to the value `max-interval` according to the following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="dda91-369">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-369">Usage</span></span>  
 <span data-ttu-id="dda91-370">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="dda91-370">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="dda91-371">Všimněte si, že podřízená omezení použití zásad zdědí tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="dda91-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="dda91-372">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-373">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-374"><a name="ReturnResponse"></a>Vrátí odpověď</span><span class="sxs-lookup"><span data-stu-id="dda91-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="dda91-375">`return-response` Zásady zruší provádění zřetězeného příkazu a vrací výchozí nebo vlastní odpověď na volajícího.</span><span class="sxs-lookup"><span data-stu-id="dda91-375">The `return-response` policy aborts pipeline execution and returns either a default or custom response to the caller.</span></span> <span data-ttu-id="dda91-376">Výchozí odpověď je `200 OK` se žádné textem.</span><span class="sxs-lookup"><span data-stu-id="dda91-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="dda91-377">Vlastní odpovědi lze zadat pomocí kontextu proměnné nebo zásady příkazy.</span><span class="sxs-lookup"><span data-stu-id="dda91-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="dda91-378">Pokud jsou obě k dispozici, odpověď obsažené v kontextové proměnné je upravit příkazy zásad před vrácením volajícímu.</span><span class="sxs-lookup"><span data-stu-id="dda91-378">When both are provided, the response contained within the context variable is modified by the policy statements before being returned to the caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-379">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-380">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-381">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-381">Elements</span></span>  
  
|<span data-ttu-id="dda91-382">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-382">Element</span></span>|<span data-ttu-id="dda91-383">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-383">Description</span></span>|<span data-ttu-id="dda91-384">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-385">vrátí odpověď</span><span class="sxs-lookup"><span data-stu-id="dda91-385">return-response</span></span>|<span data-ttu-id="dda91-386">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-386">Root element.</span></span>|<span data-ttu-id="dda91-387">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-387">Yes</span></span>|  
|<span data-ttu-id="dda91-388">set – hlavička</span><span class="sxs-lookup"><span data-stu-id="dda91-388">set-header</span></span>|<span data-ttu-id="dda91-389">A [sadu hlaviček](api-management-transformation-policies.md#SetHTTPheader) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="dda91-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="dda91-390">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-390">No</span></span>|  
|<span data-ttu-id="dda91-391">Sada textu</span><span class="sxs-lookup"><span data-stu-id="dda91-391">set-body</span></span>|<span data-ttu-id="dda91-392">A [set textu](api-management-transformation-policies.md#SetBody) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="dda91-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="dda91-393">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-393">No</span></span>|  
|<span data-ttu-id="dda91-394">Nastavit stav</span><span class="sxs-lookup"><span data-stu-id="dda91-394">set-status</span></span>|<span data-ttu-id="dda91-395">A [nastavit stav](api-management-advanced-policies.md#SetStatus) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="dda91-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="dda91-396">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-397">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-397">Attributes</span></span>  
  
|<span data-ttu-id="dda91-398">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-398">Attribute</span></span>|<span data-ttu-id="dda91-399">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-399">Description</span></span>|<span data-ttu-id="dda91-400">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="dda91-401">Název proměnné odpovědi</span><span class="sxs-lookup"><span data-stu-id="dda91-401">response-variable-name</span></span>|<span data-ttu-id="dda91-402">Název kontextové proměnné na něj odkazovat z, například předcházejícího [odeslán požadavek](api-management-advanced-policies.md#SendRequest) zásady a obsahující `Response` objektu</span><span class="sxs-lookup"><span data-stu-id="dda91-402">The name of the context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="dda91-403">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="dda91-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-404">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-404">Usage</span></span>  
 <span data-ttu-id="dda91-405">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-405">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-406">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-407">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-408"><a name="SendOneWayRequest"></a>Odeslání žádosti o jednorázové jednoduché</span><span class="sxs-lookup"><span data-stu-id="dda91-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="dda91-409">`send-one-way-request` Zásad odešle zadaný požadavek na zadanou adresu URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="dda91-409">The `send-one-way-request` policy sends the provided request to the specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-410">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-411">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-411">Example</span></span>  
 <span data-ttu-id="dda91-412">Tato ukázková zásada ukazuje příklad použití `send-one-way-request` zásad k odeslání zprávy do Slack chatovací místnosti, pokud kód odpovědi HTTP je větší než nebo rovna hodnotě 500.</span><span class="sxs-lookup"><span data-stu-id="dda91-412">This sample policy shows an example of using the `send-one-way-request` policy to send a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="dda91-413">Další informace o této ukázky najdete v tématu [pomocí externích služeb ze služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="dda91-413">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-414">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-414">Elements</span></span>  
  
|<span data-ttu-id="dda91-415">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-415">Element</span></span>|<span data-ttu-id="dda91-416">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-416">Description</span></span>|<span data-ttu-id="dda91-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-418">odeslání jeden způsob požadavků</span><span class="sxs-lookup"><span data-stu-id="dda91-418">send-one-way-request</span></span>|<span data-ttu-id="dda91-419">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-419">Root element.</span></span>|<span data-ttu-id="dda91-420">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-420">Yes</span></span>|  
|<span data-ttu-id="dda91-421">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="dda91-421">url</span></span>|<span data-ttu-id="dda91-422">Adresa URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-422">The URL of the request.</span></span>|<span data-ttu-id="dda91-423">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="dda91-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="dda91-424">– Metoda</span><span class="sxs-lookup"><span data-stu-id="dda91-424">method</span></span>|<span data-ttu-id="dda91-425">Metoda HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="dda91-425">The HTTP method for the request.</span></span>|<span data-ttu-id="dda91-426">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="dda91-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="dda91-427">záhlaví</span><span class="sxs-lookup"><span data-stu-id="dda91-427">header</span></span>|<span data-ttu-id="dda91-428">Hlavička požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-428">Request header.</span></span> <span data-ttu-id="dda91-429">Použijte více prvky záhlaví pro více hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="dda91-430">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-430">No</span></span>|  
|<span data-ttu-id="dda91-431">Text</span><span class="sxs-lookup"><span data-stu-id="dda91-431">body</span></span>|<span data-ttu-id="dda91-432">Datová část požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-432">The request body.</span></span>|<span data-ttu-id="dda91-433">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-434">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-434">Attributes</span></span>  
  
|<span data-ttu-id="dda91-435">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-435">Attribute</span></span>|<span data-ttu-id="dda91-436">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-436">Description</span></span>|<span data-ttu-id="dda91-437">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-437">Required</span></span>|<span data-ttu-id="dda91-438">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-439">režim = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-439">mode="string"</span></span>|<span data-ttu-id="dda91-440">Určuje, jestli je nový požadavek nebo kopii aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-440">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="dda91-441">V odchozí režim režim = kopie neinicializuje textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-441">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="dda91-442">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-442">No</span></span>|<span data-ttu-id="dda91-443">Nový</span><span class="sxs-lookup"><span data-stu-id="dda91-443">New</span></span>|  
|<span data-ttu-id="dda91-444">jméno</span><span class="sxs-lookup"><span data-stu-id="dda91-444">name</span></span>|<span data-ttu-id="dda91-445">Určuje název záhlaví nastavit.</span><span class="sxs-lookup"><span data-stu-id="dda91-445">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="dda91-446">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-446">Yes</span></span>|<span data-ttu-id="dda91-447">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-447">N/A</span></span>|  
|<span data-ttu-id="dda91-448">existuje akce</span><span class="sxs-lookup"><span data-stu-id="dda91-448">exists-action</span></span>|<span data-ttu-id="dda91-449">Určuje, jaká opatření se mají provést, pokud hlavička byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="dda91-449">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="dda91-450">Tento atribut musí mít jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="dda91-450">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="dda91-451">-override - nahradí hodnotu existující záhlaví.</span><span class="sxs-lookup"><span data-stu-id="dda91-451">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="dda91-452">-skip - nenahrazuje existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="dda91-452">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="dda91-453">-připojit - připojí hodnotu pro existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="dda91-453">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="dda91-454">-delete - odstraní hlavičku ze žádosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-454">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="dda91-455">Pokud nastavíte hodnotu `override` uvedení více položek se stejným názvem výsledků v hlavičce je nastavena podle všech položek (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku.</span><span class="sxs-lookup"><span data-stu-id="dda91-455">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="dda91-456">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-456">No</span></span>|<span data-ttu-id="dda91-457">přepsání</span><span class="sxs-lookup"><span data-stu-id="dda91-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-458">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-458">Usage</span></span>  
 <span data-ttu-id="dda91-459">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-459">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-460">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-461">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-462"><a name="SendRequest"></a>Odeslání požadavku</span><span class="sxs-lookup"><span data-stu-id="dda91-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="dda91-463">`send-request` Zásad odešle zadaný požadavek na zadanou adresu URL, čekání déle než nastavte hodnotu časového limitu.</span><span class="sxs-lookup"><span data-stu-id="dda91-463">The `send-request` policy sends the provided request to the specified URL, waiting no longer than the set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-464">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-465">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-465">Example</span></span>  
 <span data-ttu-id="dda91-466">Tento příklad ukazuje jeden způsob, jak ověřit odkaz tokenu se serverem ověřování.</span><span class="sxs-lookup"><span data-stu-id="dda91-466">This example shows one way to verify a reference token with an authorization server.</span></span> <span data-ttu-id="dda91-467">Další informace o této ukázky najdete v tématu [pomocí externích služeb ze služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="dda91-467">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request to Token Server to validate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-468">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-468">Elements</span></span>  
  
|<span data-ttu-id="dda91-469">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-469">Element</span></span>|<span data-ttu-id="dda91-470">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-470">Description</span></span>|<span data-ttu-id="dda91-471">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-472">odeslání žádosti</span><span class="sxs-lookup"><span data-stu-id="dda91-472">send-request</span></span>|<span data-ttu-id="dda91-473">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-473">Root element.</span></span>|<span data-ttu-id="dda91-474">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-474">Yes</span></span>|  
|<span data-ttu-id="dda91-475">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="dda91-475">url</span></span>|<span data-ttu-id="dda91-476">Adresa URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-476">The URL of the request.</span></span>|<span data-ttu-id="dda91-477">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="dda91-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="dda91-478">– Metoda</span><span class="sxs-lookup"><span data-stu-id="dda91-478">method</span></span>|<span data-ttu-id="dda91-479">Metoda HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="dda91-479">The HTTP method for the request.</span></span>|<span data-ttu-id="dda91-480">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="dda91-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="dda91-481">záhlaví</span><span class="sxs-lookup"><span data-stu-id="dda91-481">header</span></span>|<span data-ttu-id="dda91-482">Hlavička požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-482">Request header.</span></span> <span data-ttu-id="dda91-483">Použijte více prvky záhlaví pro více hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="dda91-484">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-484">No</span></span>|  
|<span data-ttu-id="dda91-485">Text</span><span class="sxs-lookup"><span data-stu-id="dda91-485">body</span></span>|<span data-ttu-id="dda91-486">Datová část požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-486">The request body.</span></span>|<span data-ttu-id="dda91-487">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-488">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-488">Attributes</span></span>  
  
|<span data-ttu-id="dda91-489">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-489">Attribute</span></span>|<span data-ttu-id="dda91-490">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-490">Description</span></span>|<span data-ttu-id="dda91-491">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-491">Required</span></span>|<span data-ttu-id="dda91-492">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-493">režim = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-493">mode="string"</span></span>|<span data-ttu-id="dda91-494">Určuje, jestli je nový požadavek nebo kopii aktuálního požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-494">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="dda91-495">V odchozí režim režim = kopie neinicializuje textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="dda91-495">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="dda91-496">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-496">No</span></span>|<span data-ttu-id="dda91-497">Nový</span><span class="sxs-lookup"><span data-stu-id="dda91-497">New</span></span>|  
|<span data-ttu-id="dda91-498">Název proměnné odpovědi = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-498">response-variable-name="string"</span></span>|<span data-ttu-id="dda91-499">Pokud není přítomný, `context.Response` se používá.</span><span class="sxs-lookup"><span data-stu-id="dda91-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="dda91-500">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-500">No</span></span>|<span data-ttu-id="dda91-501">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-501">N/A</span></span>|  
|<span data-ttu-id="dda91-502">časový limit = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="dda91-502">timeout="integer"</span></span>|<span data-ttu-id="dda91-503">Interval vypršení časového limitu v sekundách, než volání na adresu URL se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="dda91-503">The timeout interval in seconds before the call to the URL fails.</span></span>|<span data-ttu-id="dda91-504">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-504">No</span></span>|<span data-ttu-id="dda91-505">60</span><span class="sxs-lookup"><span data-stu-id="dda91-505">60</span></span>|  
|<span data-ttu-id="dda91-506">Ignorovat chybu</span><span class="sxs-lookup"><span data-stu-id="dda91-506">ignore-error</span></span>|<span data-ttu-id="dda91-507">Pokud hodnotu true a dojde k chybě žádost:</span><span class="sxs-lookup"><span data-stu-id="dda91-507">If true and the request results in an error:</span></span><br /><br /> <span data-ttu-id="dda91-508">– Pokud je název proměnné odpověď byla zadána, bude obsahovat hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="dda91-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="dda91-509">– Pokud nebyl zadán název proměnné odpovědi, kontextu. Požadavek nebude aktualizován.</span><span class="sxs-lookup"><span data-stu-id="dda91-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="dda91-510">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-510">No</span></span>|<span data-ttu-id="dda91-511">False</span><span class="sxs-lookup"><span data-stu-id="dda91-511">false</span></span>|  
|<span data-ttu-id="dda91-512">jméno</span><span class="sxs-lookup"><span data-stu-id="dda91-512">name</span></span>|<span data-ttu-id="dda91-513">Určuje název záhlaví nastavit.</span><span class="sxs-lookup"><span data-stu-id="dda91-513">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="dda91-514">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-514">Yes</span></span>|<span data-ttu-id="dda91-515">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-515">N/A</span></span>|  
|<span data-ttu-id="dda91-516">existuje akce</span><span class="sxs-lookup"><span data-stu-id="dda91-516">exists-action</span></span>|<span data-ttu-id="dda91-517">Určuje, jaká opatření se mají provést, pokud hlavička byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="dda91-517">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="dda91-518">Tento atribut musí mít jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="dda91-518">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="dda91-519">-override - nahradí hodnotu existující záhlaví.</span><span class="sxs-lookup"><span data-stu-id="dda91-519">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="dda91-520">-skip - nenahrazuje existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="dda91-520">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="dda91-521">-připojit - připojí hodnotu pro existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="dda91-521">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="dda91-522">-delete - odstraní hlavičku ze žádosti.</span><span class="sxs-lookup"><span data-stu-id="dda91-522">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="dda91-523">Pokud nastavíte hodnotu `override` uvedení více položek se stejným názvem výsledků v hlavičce je nastavena podle všech položek (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku.</span><span class="sxs-lookup"><span data-stu-id="dda91-523">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="dda91-524">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-524">No</span></span>|<span data-ttu-id="dda91-525">přepsání</span><span class="sxs-lookup"><span data-stu-id="dda91-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-526">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-526">Usage</span></span>  
 <span data-ttu-id="dda91-527">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-527">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-528">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-529">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-530"><a name="SetHttpProxy"></a>Server proxy protokolu HTTP sady</span><span class="sxs-lookup"><span data-stu-id="dda91-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="dda91-531">`proxy` Zásad umožňuje směrování požadavků předávaných do back-EndY prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="dda91-531">The `proxy` policy allows you to route requests forwarded to backends via an HTTP proxy.</span></span> <span data-ttu-id="dda91-532">Mezi brány a proxy je podporována jen protokol HTTP (nikoli HTTPS).</span><span class="sxs-lookup"><span data-stu-id="dda91-532">Only HTTP (not HTTPS) is supported between the gateway and the proxy.</span></span> <span data-ttu-id="dda91-533">Pouze ověřování NTLM a Basic.</span><span class="sxs-lookup"><span data-stu-id="dda91-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-534">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-535">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-535">Example</span></span>  
<span data-ttu-id="dda91-536">Všimněte si použití [vlastnosti](api-management-howto-properties.md) jako hodnoty uživatelské jméno a heslo, aby se zabránilo ukládání citlivých informací v dokumentu zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-536">Note the use of [properties](api-management-howto-properties.md) as values of the username and password to avoid storing sensitive information in the policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-537">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-537">Elements</span></span>  
  
|<span data-ttu-id="dda91-538">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-538">Element</span></span>|<span data-ttu-id="dda91-539">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-539">Description</span></span>|<span data-ttu-id="dda91-540">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-541">Proxy server</span><span class="sxs-lookup"><span data-stu-id="dda91-541">proxy</span></span>|<span data-ttu-id="dda91-542">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="dda91-542">Root element</span></span>|<span data-ttu-id="dda91-543">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="dda91-544">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-544">Attributes</span></span>  
  
|<span data-ttu-id="dda91-545">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-545">Attribute</span></span>|<span data-ttu-id="dda91-546">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-546">Description</span></span>|<span data-ttu-id="dda91-547">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-547">Required</span></span>|<span data-ttu-id="dda91-548">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-549">Adresa URL = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-549">url="string"</span></span>|<span data-ttu-id="dda91-550">Adresa URL proxy serveru ve formě http://host:port.</span><span class="sxs-lookup"><span data-stu-id="dda91-550">Proxy URL in the form of http://host:port.</span></span>|<span data-ttu-id="dda91-551">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-551">Yes</span></span>|<span data-ttu-id="dda91-552">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-552">N/A</span></span>|  
|<span data-ttu-id="dda91-553">uživatelské jméno = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-553">username="string"</span></span>|<span data-ttu-id="dda91-554">Uživatelské jméno má být použit pro ověřování s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="dda91-554">Username to be used for authentication with the proxy.</span></span>|<span data-ttu-id="dda91-555">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-555">No</span></span>|<span data-ttu-id="dda91-556">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-556">N/A</span></span>|  
|<span data-ttu-id="dda91-557">heslo = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-557">password="string"</span></span>|<span data-ttu-id="dda91-558">Heslo má být použit pro ověřování s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="dda91-558">Password to be used for authentication with the proxy.</span></span>|<span data-ttu-id="dda91-559">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-559">No</span></span>|<span data-ttu-id="dda91-560">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="dda91-561">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-561">Usage</span></span>  
 <span data-ttu-id="dda91-562">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-562">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-563">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="dda91-564">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="dda91-565"><a name="SetRequestMethod"></a>Žádost o set – metoda</span><span class="sxs-lookup"><span data-stu-id="dda91-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="dda91-566">`set-method` Zásad vám umožní změnit metodu požadavku HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="dda91-566">The `set-method` policy allows you to change the HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-567">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-568">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-568">Example</span></span>  
 <span data-ttu-id="dda91-569">Tato ukázka zásadu, která používá `set-method` zásad ukazuje příklad odesílání zprávy Slack chatovací místnosti, pokud kód odpovědi HTTP je větší než nebo rovna hodnotě 500.</span><span class="sxs-lookup"><span data-stu-id="dda91-569">This sample policy that uses the `set-method` policy shows an example of sending a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="dda91-570">Další informace o této ukázky najdete v tématu [pomocí externích služeb ze služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="dda91-570">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-571">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-571">Elements</span></span>  
  
|<span data-ttu-id="dda91-572">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-572">Element</span></span>|<span data-ttu-id="dda91-573">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-573">Description</span></span>|<span data-ttu-id="dda91-574">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-575">set – metoda</span><span class="sxs-lookup"><span data-stu-id="dda91-575">set-method</span></span>|<span data-ttu-id="dda91-576">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-576">Root element.</span></span> <span data-ttu-id="dda91-577">Hodnota elementu určuje metodu HTTP.</span><span class="sxs-lookup"><span data-stu-id="dda91-577">The value of the element specifies the HTTP method.</span></span>|<span data-ttu-id="dda91-578">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-579">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-579">Usage</span></span>  
 <span data-ttu-id="dda91-580">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-580">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-581">**Části zásady:** příchozí, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="dda91-582">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-583"><a name="SetStatus"></a>Sada stavový kód</span><span class="sxs-lookup"><span data-stu-id="dda91-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="dda91-584">`set-status` Zásady nastaví stavový kód protokolu HTTP se zadanou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="dda91-584">The `set-status` policy sets the HTTP status code to the specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-585">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-586">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-586">Example</span></span>  
 <span data-ttu-id="dda91-587">Tento příklad ukazuje, jak vracet odpovědi 401, pokud autorizační token je neplatný.</span><span class="sxs-lookup"><span data-stu-id="dda91-587">This example shows how to return a 401 response if the authorization token is invalid.</span></span> <span data-ttu-id="dda91-588">Další informace najdete v tématu [pomocí externích služeb ze služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="dda91-588">For more information, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-589">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-589">Elements</span></span>  
  
|<span data-ttu-id="dda91-590">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-590">Element</span></span>|<span data-ttu-id="dda91-591">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-591">Description</span></span>|<span data-ttu-id="dda91-592">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-593">Nastavit stav</span><span class="sxs-lookup"><span data-stu-id="dda91-593">set-status</span></span>|<span data-ttu-id="dda91-594">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-594">Root element.</span></span>|<span data-ttu-id="dda91-595">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-596">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-596">Attributes</span></span>  
  
|<span data-ttu-id="dda91-597">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-597">Attribute</span></span>|<span data-ttu-id="dda91-598">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-598">Description</span></span>|<span data-ttu-id="dda91-599">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-599">Required</span></span>|<span data-ttu-id="dda91-600">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-601">kód = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="dda91-601">code="integer"</span></span>|<span data-ttu-id="dda91-602">Stavový kód HTTP vrátit.</span><span class="sxs-lookup"><span data-stu-id="dda91-602">The HTTP status code to return.</span></span>|<span data-ttu-id="dda91-603">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-603">Yes</span></span>|<span data-ttu-id="dda91-604">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-604">N/A</span></span>|  
|<span data-ttu-id="dda91-605">důvod = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="dda91-605">reason="string"</span></span>|<span data-ttu-id="dda91-606">Popis důvod pro vrácení stavový kód.</span><span class="sxs-lookup"><span data-stu-id="dda91-606">A description of the reason for returning the status code.</span></span>|<span data-ttu-id="dda91-607">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-607">Yes</span></span>|<span data-ttu-id="dda91-608">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-609">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-609">Usage</span></span>  
 <span data-ttu-id="dda91-610">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-610">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-611">**Části zásady:** odchozí, back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-612">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="dda91-613"><a name="set-variable"></a>Nastavená proměnná</span><span class="sxs-lookup"><span data-stu-id="dda91-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="dda91-614">`set-variable` Deklaruje zásad [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné a přiřadí ji má hodnotu prostřednictvím [výraz](api-management-policy-expressions.md) nebo řetězcový literál.</span><span class="sxs-lookup"><span data-stu-id="dda91-614">The `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="dda91-615">Pokud výraz obsahuje literál jej bude možné převést na řetězec a typ hodnoty bude `System.String`.</span><span class="sxs-lookup"><span data-stu-id="dda91-615">if the expression contains a literal it will be converted to a string and the type of the value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="dda91-616"><a name="set-variablePolicyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="dda91-617"><a name="set-variableExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="dda91-618">Následující příklad ukazuje nastavení proměnné zásad v části příchozí.</span><span class="sxs-lookup"><span data-stu-id="dda91-618">The following example demonstrates a set variable policy in the inbound section.</span></span> <span data-ttu-id="dda91-619">Toto nastavení zásad proměnné vytvoří `isMobile` logickou [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena na hodnotu true, pokud `User-Agent` žádosti záhlaví obsahuje text `iPad` nebo `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="dda91-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-620">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-620">Elements</span></span>  
  
|<span data-ttu-id="dda91-621">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-621">Element</span></span>|<span data-ttu-id="dda91-622">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-622">Description</span></span>|<span data-ttu-id="dda91-623">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-624">nastavená proměnná</span><span class="sxs-lookup"><span data-stu-id="dda91-624">set-variable</span></span>|<span data-ttu-id="dda91-625">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-625">Root element.</span></span>|<span data-ttu-id="dda91-626">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-627">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-627">Attributes</span></span>  
  
|<span data-ttu-id="dda91-628">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-628">Attribute</span></span>|<span data-ttu-id="dda91-629">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-629">Description</span></span>|<span data-ttu-id="dda91-630">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="dda91-631">jméno</span><span class="sxs-lookup"><span data-stu-id="dda91-631">name</span></span>|<span data-ttu-id="dda91-632">Název proměnné.</span><span class="sxs-lookup"><span data-stu-id="dda91-632">The name of the variable.</span></span>|<span data-ttu-id="dda91-633">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-633">Yes</span></span>|  
|<span data-ttu-id="dda91-634">hodnota</span><span class="sxs-lookup"><span data-stu-id="dda91-634">value</span></span>|<span data-ttu-id="dda91-635">Hodnota proměnné.</span><span class="sxs-lookup"><span data-stu-id="dda91-635">The value of the variable.</span></span> <span data-ttu-id="dda91-636">To může být výraz nebo hodnota literálu.</span><span class="sxs-lookup"><span data-stu-id="dda91-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="dda91-637">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-638">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-638">Usage</span></span>  
 <span data-ttu-id="dda91-639">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-639">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-640">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-641">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="dda91-642"><a name="set-variableAllowedTypes"></a>Povolené typy</span><span class="sxs-lookup"><span data-stu-id="dda91-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="dda91-643">Výrazy použité v `set-variable` zásady musí vrátit jednu z následujících základních typů.</span><span class="sxs-lookup"><span data-stu-id="dda91-643">Expressions used in the `set-variable` policy must return one of the following basic types.</span></span>  
  
-   <span data-ttu-id="dda91-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="dda91-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="dda91-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="dda91-645">System.SByte</span></span>  
  
-   <span data-ttu-id="dda91-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="dda91-646">System.Byte</span></span>  
  
-   <span data-ttu-id="dda91-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="dda91-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="dda91-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="dda91-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="dda91-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="dda91-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="dda91-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="dda91-650">System.Int16</span></span>  
  
-   <span data-ttu-id="dda91-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="dda91-651">System.Int32</span></span>  
  
-   <span data-ttu-id="dda91-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="dda91-652">System.Int64</span></span>  
  
-   <span data-ttu-id="dda91-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="dda91-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="dda91-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="dda91-654">System.Single</span></span>  
  
-   <span data-ttu-id="dda91-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="dda91-655">System.Double</span></span>  
  
-   <span data-ttu-id="dda91-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="dda91-656">System.Guid</span></span>  
  
-   <span data-ttu-id="dda91-657">System.String</span><span class="sxs-lookup"><span data-stu-id="dda91-657">System.String</span></span>  
  
-   <span data-ttu-id="dda91-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="dda91-658">System.Char</span></span>  
  
-   <span data-ttu-id="dda91-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="dda91-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="dda91-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="dda91-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="dda91-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="dda91-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="dda91-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="dda91-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="dda91-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="dda91-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="dda91-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="dda91-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="dda91-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="dda91-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="dda91-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="dda91-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="dda91-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="dda91-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="dda91-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="dda91-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="dda91-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="dda91-669">System.Single?</span></span>  
  
-   <span data-ttu-id="dda91-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="dda91-670">System.Double?</span></span>  
  
-   <span data-ttu-id="dda91-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="dda91-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="dda91-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="dda91-672">System.String?</span></span>  
  
-   <span data-ttu-id="dda91-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="dda91-673">System.Char?</span></span>  
  
-   <span data-ttu-id="dda91-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="dda91-674">System.DateTime?</span></span>  

##  <span data-ttu-id="dda91-675"><a name="Trace"></a>Trasování</span><span class="sxs-lookup"><span data-stu-id="dda91-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="dda91-676">`trace` Zásad přidá řetězec do [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="dda91-676">The             `trace` policy adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="dda91-677">Zásady, které budou spuštěny pouze při trasování je aktivaci, tj. `Ocp-Apim-Trace` hlavička požadavku je přítomen a nastavené na `true` a `Ocp-Apim-Subscription-Key` hlavička požadavku je k dispozici a obsahuje platný klíč přidružený k účtu správce.</span><span class="sxs-lookup"><span data-stu-id="dda91-677">The policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set to `true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with the admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-678">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-679">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-679">Elements</span></span>  
  
|<span data-ttu-id="dda91-680">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-680">Element</span></span>|<span data-ttu-id="dda91-681">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-681">Description</span></span>|<span data-ttu-id="dda91-682">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-683">trasování</span><span class="sxs-lookup"><span data-stu-id="dda91-683">trace</span></span>|<span data-ttu-id="dda91-684">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-684">Root element.</span></span>|<span data-ttu-id="dda91-685">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-686">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-686">Attributes</span></span>  
  
|<span data-ttu-id="dda91-687">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-687">Attribute</span></span>|<span data-ttu-id="dda91-688">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-688">Description</span></span>|<span data-ttu-id="dda91-689">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-689">Required</span></span>|<span data-ttu-id="dda91-690">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-691">Zdroj</span><span class="sxs-lookup"><span data-stu-id="dda91-691">source</span></span>|<span data-ttu-id="dda91-692">Řetězcový literál smysl prohlížeče trasování a zadání zdroj zprávy.</span><span class="sxs-lookup"><span data-stu-id="dda91-692">String literal meaningful to the trace viewer and specifying the source of the message.</span></span>|<span data-ttu-id="dda91-693">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-693">Yes</span></span>|<span data-ttu-id="dda91-694">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="dda91-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-695">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-695">Usage</span></span>  
 <span data-ttu-id="dda91-696">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="dda91-696">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="dda91-697">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="dda91-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="dda91-698">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="dda91-699"><a name="Wait"></a>Počkej</span><span class="sxs-lookup"><span data-stu-id="dda91-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="dda91-700">`wait` Zásady spustí své zásady bezprostředně podřízené paralelně a čeká na všech nebo jednoho z jeho bezprostředně podřízené zásady pro provedení úkolu před jeho dokončení.</span><span class="sxs-lookup"><span data-stu-id="dda91-700">The `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies to complete before it completes.</span></span> <span data-ttu-id="dda91-701">Počkejte zásad může mít jako jeho bezprostředně podřízené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), a [řízení toku](api-management-advanced-policies.md#choose) zásady.</span><span class="sxs-lookup"><span data-stu-id="dda91-701">The wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="dda91-702">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="dda91-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="dda91-703">Příklad</span><span class="sxs-lookup"><span data-stu-id="dda91-703">Example</span></span>  
 <span data-ttu-id="dda91-704">V následujícím příkladu jsou uvedeny dvě `choose` zásady jako bezprostředně podřízené zásady `wait` zásad.</span><span class="sxs-lookup"><span data-stu-id="dda91-704">In the following example there are two `choose` policies as immediate child policies of the `wait` policy.</span></span> <span data-ttu-id="dda91-705">Každá z těchto `choose` zásady spustí paralelně.</span><span class="sxs-lookup"><span data-stu-id="dda91-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="dda91-706">Každý `choose` zásady, pokusí se načíst hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="dda91-706">Each `choose` policy attempts to retrieve a cached value.</span></span> <span data-ttu-id="dda91-707">Pokud dojde k neúspěšnému přístupu do mezipaměti, se nazývá back-end službu do zadejte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dda91-707">If there is a cache miss, a backend service is called to provide the value.</span></span> <span data-ttu-id="dda91-708">V tomto příkladu `wait` zásad dokončena, dokud se všechny jeho podřízené okamžitou zásady dokončit, protože `for` je nastavena na hodnotu `all`.</span><span class="sxs-lookup"><span data-stu-id="dda91-708">In this example the `wait` policy does not complete until all of its immediate child policies complete, because the `for` attribute is set to `all`.</span></span>   <span data-ttu-id="dda91-709">V tomto příkladu kontextu proměnné (`execute-branch-one`, `value-one`, `execute-branch-two`, a `value-two`) jsou deklarované mimo obor Tento příklad zásady.</span><span class="sxs-lookup"><span data-stu-id="dda91-709">In this example the context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of the scope of this example policy.</span></span>  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="dda91-710">Elementy</span><span class="sxs-lookup"><span data-stu-id="dda91-710">Elements</span></span>  
  
|<span data-ttu-id="dda91-711">Element</span><span class="sxs-lookup"><span data-stu-id="dda91-711">Element</span></span>|<span data-ttu-id="dda91-712">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-712">Description</span></span>|<span data-ttu-id="dda91-713">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="dda91-714">Počkej</span><span class="sxs-lookup"><span data-stu-id="dda91-714">wait</span></span>|<span data-ttu-id="dda91-715">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="dda91-715">Root element.</span></span> <span data-ttu-id="dda91-716">Může obsahovat pouze podřízené elementy `send-request`, `cache-lookup-value`, a `choose` zásady.</span><span class="sxs-lookup"><span data-stu-id="dda91-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="dda91-717">Ano</span><span class="sxs-lookup"><span data-stu-id="dda91-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="dda91-718">Atributy</span><span class="sxs-lookup"><span data-stu-id="dda91-718">Attributes</span></span>  
  
|<span data-ttu-id="dda91-719">Atribut</span><span class="sxs-lookup"><span data-stu-id="dda91-719">Attribute</span></span>|<span data-ttu-id="dda91-720">Popis</span><span class="sxs-lookup"><span data-stu-id="dda91-720">Description</span></span>|<span data-ttu-id="dda91-721">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="dda91-721">Required</span></span>|<span data-ttu-id="dda91-722">Výchozí</span><span class="sxs-lookup"><span data-stu-id="dda91-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="dda91-723">Pro</span><span class="sxs-lookup"><span data-stu-id="dda91-723">for</span></span>|<span data-ttu-id="dda91-724">Určuje, zda `wait` zásad čeká na všechny zásady bezprostředně podřízené jako dokončená nebo jenom jeden.</span><span class="sxs-lookup"><span data-stu-id="dda91-724">Determines whether the `wait` policy waits for all immediate child policies to be completed or just one.</span></span> <span data-ttu-id="dda91-725">Povolené hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="dda91-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="dda91-726">-   `all`-čekat na všechny zásady bezprostředně podřízené k dokončení</span><span class="sxs-lookup"><span data-stu-id="dda91-726">-   `all` - wait for all immediate child policies to complete</span></span><br /><span data-ttu-id="dda91-727">-všechny - počkejte všechny bezprostředně podřízené zásadu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="dda91-727">-   any - wait for any immediate child policy to complete.</span></span> <span data-ttu-id="dda91-728">Po dokončení prvního bezprostředně podřízené zásad `wait` zásad dokončí a provádění dalších zásad bezprostředně podřízené je ukončen.</span><span class="sxs-lookup"><span data-stu-id="dda91-728">Once the first immediate child policy has completed, the `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="dda91-729">Ne</span><span class="sxs-lookup"><span data-stu-id="dda91-729">No</span></span>|<span data-ttu-id="dda91-730">Všechny</span><span class="sxs-lookup"><span data-stu-id="dda91-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="dda91-731">Využití</span><span class="sxs-lookup"><span data-stu-id="dda91-731">Usage</span></span>  
 <span data-ttu-id="dda91-732">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="dda91-732">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="dda91-733">**Části zásady:** vstupní, výstupní a back-end</span><span class="sxs-lookup"><span data-stu-id="dda91-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="dda91-734">**Zásady obory:**všechny obory</span><span class="sxs-lookup"><span data-stu-id="dda91-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="dda91-735">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dda91-735">Next steps</span></span>
<span data-ttu-id="dda91-736">Práce se zásadami pro další informace najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="dda91-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="dda91-737">Zásady ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="dda91-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="dda91-738">Výrazy zásad</span><span class="sxs-lookup"><span data-stu-id="dda91-738">Policy expressions</span></span>](api-management-policy-expressions.md)
