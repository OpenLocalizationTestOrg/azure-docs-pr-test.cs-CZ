---
title: "aaaAzure pokročilé zásady API Management | Microsoft Docs"
description: "Další informace o hello pokročilé zásady, které jsou k dispozici pro použití v Azure API Management."
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
ms.openlocfilehash: 8245e7a4c9d432b7b4d362192e357829fcabad55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-advanced-policies"></a><span data-ttu-id="70443-103">Pokročilé zásady API Management</span><span class="sxs-lookup"><span data-stu-id="70443-103">API Management advanced policies</span></span>
<span data-ttu-id="70443-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="70443-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="70443-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="70443-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="70443-106"><a name="AdvancedPolicies"></a>Rozšířené zásady</span><span class="sxs-lookup"><span data-stu-id="70443-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="70443-107">[Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad na základě výsledků hello hello vyhodnocení logická hodnota [výrazy](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="70443-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="70443-108">[Předat dál žádost](#ForwardRequest) -předává hello požadavek toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="70443-108">[Forward request](#ForwardRequest) - Forwards hello request toohello backend service.</span></span>

-   <span data-ttu-id="70443-109">[Omezit souběžnosti](#LimitConcurrency) -brání uzavřena zásady z provádění více než hello určitý počet požadavků současně.</span><span class="sxs-lookup"><span data-stu-id="70443-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than hello specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="70443-110">[Protokolu tooEvent rozbočovače](#log-to-eventhub) -odešle zprávy v hello zadaný formát tooan centra událostí definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="70443-110">[Log tooEvent Hub](#log-to-eventhub) - Sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="70443-111">[Model odpovědi](#mock-response) -přerušení způsobených kanálu provádění a vrátí odpověď mocked přímo toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="70443-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly toohello caller.</span></span>
  
-   <span data-ttu-id="70443-112">[Opakujte](#Retry) -provádění opakování hello uzavřené příkazy zásad, pokud a dokud je splněna podmínka hello.</span><span class="sxs-lookup"><span data-stu-id="70443-112">[Retry](#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="70443-113">Spuštění se bude opakovat v hello zadaným časovým intervalům a až toohello zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="70443-113">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
-   <span data-ttu-id="70443-114">[Vrátí odpověď](#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí hello zadané odpovědi přímo toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="70443-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span> 
  
-   <span data-ttu-id="70443-115">[Odeslání žádosti o jednorázové jednoduché](#SendOneWayRequest) -odešle žádost toohello zadané adresy URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="70443-115">[Send one way request](#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="70443-116">[Odeslání požadavku](#SendRequest) -odešle žádost toohello zadaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="70443-116">[Send request](#SendRequest) - Sends a request toohello specified URL.</span></span>  

-   <span data-ttu-id="70443-117">[Nastavení proxy serveru HTTP](#SetHttpProxy) -vám umožní tooroute předané požadavky prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="70443-117">[Set HTTP proxy](#SetHttpProxy) - Allows you tooroute forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="70443-118">[Nastaví metodu požadavku](#SetRequestMethod) -vám umožní toochange hello HTTP metoda pro žádost.</span><span class="sxs-lookup"><span data-stu-id="70443-118">[Set request method](#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="70443-119">[Nastavit stavový kód](#SetStatus) -toohello kód stavu hello HTTP změny zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="70443-119">[Set status code](#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
-   <span data-ttu-id="70443-120">[Nastavená proměnná](api-management-advanced-policies.md#set-variable) -potrvají hodnotu v pojmenovaná [kontextu](api-management-policy-expressions.md#ContextVariables) proměnná pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="70443-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="70443-121">[Trasování](#Trace) -přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="70443-121">[Trace](#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="70443-122">[Počkejte](#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady toocomplete než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="70443-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
##  <span data-ttu-id="70443-123"><a name="choose"></a>Tok řízení</span><span class="sxs-lookup"><span data-stu-id="70443-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="70443-124">Hello `choose` zásada závorkách zásad příkazy podle hello výsledek vyhodnocení logické výrazy, podobně jako v případě pak jinak tooan nebo přepínač vytvořit v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="70443-124">hello `choose` policy applies enclosed policy statements based on hello outcome of evaluation of Boolean expressions, similar tooan if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="70443-125"><a name="ChoosePolicyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements toobe applied if none of hello above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 <span data-ttu-id="70443-126">Hello zásad řízení toku musí obsahovat alespoň jeden `<when/>` elementu.</span><span class="sxs-lookup"><span data-stu-id="70443-126">hello control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="70443-127">Hello `<otherwise/>` prvek je volitelný.</span><span class="sxs-lookup"><span data-stu-id="70443-127">hello `<otherwise/>` element is optional.</span></span> <span data-ttu-id="70443-128">Podmínky v `<when/>` elementy jsou vyhodnocovány v pořadí podle jejich vzhled v rámci zásad hello.</span><span class="sxs-lookup"><span data-stu-id="70443-128">Conditions in `<when/>` elements are evaluated in order of their appearance within hello policy.</span></span> <span data-ttu-id="70443-129">Příkazy zásad uzavřené v rámci hello nejprve `<when/>` element s podmínku atributu rovná `true` se použijí.</span><span class="sxs-lookup"><span data-stu-id="70443-129">Policy statement(s) enclosed within hello first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="70443-130">Zásady v rámci hello uzavřené `<otherwise/>` elementu, pokud existuje, bude použito v případě všech z hello `<when/>` element podmínku atributy jsou `false`.</span><span class="sxs-lookup"><span data-stu-id="70443-130">Policies enclosed within hello `<otherwise/>` element, if present, will be applied if all of hello `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="70443-131">Příklady</span><span class="sxs-lookup"><span data-stu-id="70443-131">Examples</span></span>  
  
####  <span data-ttu-id="70443-132"><a name="ChooseExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="70443-133">Hello následující příklad ukazuje [nastavená proměnná](api-management-advanced-policies.md#set-variable) zásady a dvě zásady řízení toku.</span><span class="sxs-lookup"><span data-stu-id="70443-133">hello following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="70443-134">Hello nastavit proměnnou zásady je v hello příchozí části a vytvoří `isMobile` Boolean [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena tootrue, pokud hello `User-Agent` žádosti záhlaví obsahuje hello text `iPad` nebo `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="70443-134">hello set variable policy is in hello inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="70443-135">zásada toku řízení první Hello je taky v hello příchozí části a podmíněně použije jednu ze dvou [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásady v závislosti na hodnotu hello hello `isMobile` kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="70443-135">hello first control flow policy is also in hello inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on hello value of hello `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="70443-136">Hello druhý řízení toku zásady je v části odchozí hello a podmíněně platí hello [převést XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) zásady při `isMobile` je nastaven příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="70443-136">hello second control flow policy is in hello outbound section and conditionally applies hello [Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set too`true`.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="70443-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-137">Example</span></span>  
 <span data-ttu-id="70443-138">Tento příklad ukazuje, jak tooperform filtrování obsahu odebráním datové prvky. z odpovědi hello přijal od služby back-end hello při použití hello `Starter` produktu.</span><span class="sxs-lookup"><span data-stu-id="70443-138">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="70443-139">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too34:30.</span><span class="sxs-lookup"><span data-stu-id="70443-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="70443-140">Spuštění 31:50 toosee přehled [hello tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="70443-140">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-141">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-141">Elements</span></span>  
  
|<span data-ttu-id="70443-142">Element</span><span class="sxs-lookup"><span data-stu-id="70443-142">Element</span></span>|<span data-ttu-id="70443-143">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-143">Description</span></span>|<span data-ttu-id="70443-144">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-145">Zvolte</span><span class="sxs-lookup"><span data-stu-id="70443-145">choose</span></span>|<span data-ttu-id="70443-146">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-146">Root element.</span></span>|<span data-ttu-id="70443-147">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-147">Yes</span></span>|  
|<span data-ttu-id="70443-148">Kdy</span><span class="sxs-lookup"><span data-stu-id="70443-148">when</span></span>|<span data-ttu-id="70443-149">Hello toouse podmínku pro hello `if` nebo `ifelse` částí hello `choose` zásad.</span><span class="sxs-lookup"><span data-stu-id="70443-149">hello condition toouse for hello `if` or `ifelse` parts of hello `choose` policy.</span></span> <span data-ttu-id="70443-150">Pokud hello `choose` zásad obsahuje více `when` oddíly, jsou vyhodnocovány postupně.</span><span class="sxs-lookup"><span data-stu-id="70443-150">If hello `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="70443-151">Jednou hello `condition` systému v případě elementu vyhodnotí příliš`true`, žádné další `when` vyhodnocení podmínek.</span><span class="sxs-lookup"><span data-stu-id="70443-151">Once hello `condition` of a when element evaluates too`true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="70443-152">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-152">Yes</span></span>|  
|<span data-ttu-id="70443-153">v opačném případě</span><span class="sxs-lookup"><span data-stu-id="70443-153">otherwise</span></span>|<span data-ttu-id="70443-154">Obsahuje hello zásad fragment kódu toobe použít, pokud žádná z hello `when` podmínky vyhodnotit příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="70443-154">Contains hello policy snippet toobe used if none of hello `when` conditions evaluate too`true`.</span></span>|<span data-ttu-id="70443-155">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-156">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-156">Attributes</span></span>  
  
|<span data-ttu-id="70443-157">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-157">Attribute</span></span>|<span data-ttu-id="70443-158">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-158">Description</span></span>|<span data-ttu-id="70443-159">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="70443-160">podmínka = "logický výraz &#124; Logická hodnota konstanta"</span><span class="sxs-lookup"><span data-stu-id="70443-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="70443-161">logický výraz nebo konstantní tooevaluated při hello obsahující Hello `when` zásad vyhodnotí.</span><span class="sxs-lookup"><span data-stu-id="70443-161">hello Boolean expression or constant tooevaluated when hello containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="70443-162">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-162">Yes</span></span>|  
  
###  <span data-ttu-id="70443-163"><a name="ChooseUsage"></a>Využití</span><span class="sxs-lookup"><span data-stu-id="70443-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="70443-164">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-164">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-165">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-166">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-167"><a name="ForwardRequest"></a>Předat dál požadavku</span><span class="sxs-lookup"><span data-stu-id="70443-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="70443-168">Hello `forward-request` zásada předá hello příchozí požadavek toohello back-end službu určený v požadavku hello [kontextu](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="70443-168">hello `forward-request` policy forwards hello incoming request toohello backend service specified in hello request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="70443-169">Hello adresa URL back-end služby je zadaný v hello rozhraní API [nastavení](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) a můžete změnit pomocí hello [nastavení back-end služby](api-management-transformation-policies.md) zásad.</span><span class="sxs-lookup"><span data-stu-id="70443-169">hello backend service URL is specified in hello API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using hello [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="70443-170">Odebrání výsledkem požadavku hello nebudou předávány zásady služby a hello back-end toohello v části odchozí hello zásad se vyhodnocují hned po úspěšném hello hello zásad v hello příchozí části.</span><span class="sxs-lookup"><span data-stu-id="70443-170">Removing this policy results in hello request not being forwarded toohello backend service and hello policies in hello outbound section are evaluated immediately upon hello successful completion of hello policies in hello inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-171">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="70443-172">Příklady</span><span class="sxs-lookup"><span data-stu-id="70443-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="70443-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-173">Example</span></span>  
 <span data-ttu-id="70443-174">Hello následující zásadou na úrovni rozhraní API předává všechny požadavky toohello back-end službu s časový limit na 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="70443-174">hello following API level policy forwards all requests toohello backend service with a timeout interval of 60 seconds.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="70443-175">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-175">Example</span></span>  
 <span data-ttu-id="70443-176">Tato zásada na úrovni operace používá hello `base` element tooinherit hello back-end zásady z hello nadřazené API úrovni oboru.</span><span class="sxs-lookup"><span data-stu-id="70443-176">This operation level policy uses hello `base` element tooinherit hello backend policy from hello parent API level scope.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="70443-177">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-177">Example</span></span>  
 <span data-ttu-id="70443-178">Tato zásada na úrovni operaci explicitně předává všechny požadavky toohello back-end službu s časovým limitem 120 a nedědí hello nadřazené úrovně back-end zásada rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="70443-178">This operation level policy explicitly forwards all requests toohello backend service with a timeout of 120 and does not inherit hello parent API level backend policy.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note hello absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="70443-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-179">Example</span></span>  
 <span data-ttu-id="70443-180">Tato zásada na úrovni operaci nepředává požadavky toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="70443-180">This operation level policy does not forward requests toohello backend service.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding toobackend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-181">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-181">Elements</span></span>  
  
|<span data-ttu-id="70443-182">Element</span><span class="sxs-lookup"><span data-stu-id="70443-182">Element</span></span>|<span data-ttu-id="70443-183">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-183">Description</span></span>|<span data-ttu-id="70443-184">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-185">předání požadavku</span><span class="sxs-lookup"><span data-stu-id="70443-185">forward-request</span></span>|<span data-ttu-id="70443-186">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-186">Root element.</span></span>|<span data-ttu-id="70443-187">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-188">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-188">Attributes</span></span>  
  
|<span data-ttu-id="70443-189">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-189">Attribute</span></span>|<span data-ttu-id="70443-190">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-190">Description</span></span>|<span data-ttu-id="70443-191">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-191">Required</span></span>|<span data-ttu-id="70443-192">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-193">časový limit = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="70443-193">timeout="integer"</span></span>|<span data-ttu-id="70443-194">Hello interval časového limitu v sekundách, než hello volání toohello back-end službu se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="70443-194">hello timeout interval in seconds before hello call toohello backend service fails.</span></span>|<span data-ttu-id="70443-195">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-195">No</span></span>|<span data-ttu-id="70443-196">Bez časového limitu</span><span class="sxs-lookup"><span data-stu-id="70443-196">No timeout</span></span>|  
|<span data-ttu-id="70443-197">postupujte podle přesměrování = "true &#124; false"</span><span class="sxs-lookup"><span data-stu-id="70443-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="70443-198">Určuje, zda jsou přesměrování z back-end službu hello následuje hello brány nebo vrátil toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="70443-198">Specifies whether redirects from hello backend service are followed by hello gateway or returned toohello caller.</span></span>|<span data-ttu-id="70443-199">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-199">No</span></span>|<span data-ttu-id="70443-200">False</span><span class="sxs-lookup"><span data-stu-id="70443-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-201">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-201">Usage</span></span>  
 <span data-ttu-id="70443-202">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-202">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-203">**Části zásady:** back-end</span><span class="sxs-lookup"><span data-stu-id="70443-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="70443-204">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-205"><a name="LimitConcurrency"></a>Limit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="70443-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="70443-206">Hello `limit-concurrency` zásady zabrání závorkách zásady provádění více než hello zadaný počet požadavků, které v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="70443-206">hello `limit-concurrency` policy prevents enclosed policies from executing by more than hello specified number of requests at a given time.</span></span> <span data-ttu-id="70443-207">Při překročení prahové hodnoty hello, přidají nové žádosti o tooa fronty, dokud nedosáhnete hello maximální délka fronty.</span><span class="sxs-lookup"><span data-stu-id="70443-207">Upon exceeding hello threshold, new requests are added tooa queue, until hello maximum queue length is achieved.</span></span> <span data-ttu-id="70443-208">Po vyčerpání fronty nové požadavky selže okamžitě.</span><span class="sxs-lookup"><span data-stu-id="70443-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="70443-209"><a name="LimitConcurrencyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="70443-210">Příklady</span><span class="sxs-lookup"><span data-stu-id="70443-210">Examples</span></span>  
  
####  <span data-ttu-id="70443-211"><a name="ChooseExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="70443-212">Hello následující příklad ukazuje, jak toolimit počet požadavků, které přesměrovávají tooa back-end na základě hodnoty hello kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="70443-212">hello following example demonstrates how toolimit number of requests forwarded tooa backend based on hello value of a context variable.</span></span>
 
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

### <a name="elements"></a><span data-ttu-id="70443-213">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-213">Elements</span></span>  
  
|<span data-ttu-id="70443-214">Element</span><span class="sxs-lookup"><span data-stu-id="70443-214">Element</span></span>|<span data-ttu-id="70443-215">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-215">Description</span></span>|<span data-ttu-id="70443-216">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="70443-217">limit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="70443-217">limit-concurrency</span></span>|<span data-ttu-id="70443-218">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-218">Root element.</span></span>|<span data-ttu-id="70443-219">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-220">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-220">Attributes</span></span>  
  
|<span data-ttu-id="70443-221">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-221">Attribute</span></span>|<span data-ttu-id="70443-222">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-222">Description</span></span>|<span data-ttu-id="70443-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-223">Required</span></span>|<span data-ttu-id="70443-224">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="70443-225">key</span><span class="sxs-lookup"><span data-stu-id="70443-225">key</span></span>|<span data-ttu-id="70443-226">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="70443-226">A string.</span></span> <span data-ttu-id="70443-227">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="70443-227">Expression allowed.</span></span> <span data-ttu-id="70443-228">Určuje obor souběžnosti hello.</span><span class="sxs-lookup"><span data-stu-id="70443-228">Specifies hello concurrency scope.</span></span> <span data-ttu-id="70443-229">Může být sdílen více zásad.</span><span class="sxs-lookup"><span data-stu-id="70443-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="70443-230">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-230">Yes</span></span>|<span data-ttu-id="70443-231">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-231">N/A</span></span>|  
|<span data-ttu-id="70443-232">maximální počet</span><span class="sxs-lookup"><span data-stu-id="70443-232">max-count</span></span>|<span data-ttu-id="70443-233">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="70443-233">An integer.</span></span> <span data-ttu-id="70443-234">Určuje maximální počet požadavků, které jsou povoleny tooenter hello zásad.</span><span class="sxs-lookup"><span data-stu-id="70443-234">Specifies a maximum number of requests that are allowed tooenter hello policy.</span></span>|<span data-ttu-id="70443-235">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-235">Yes</span></span>|<span data-ttu-id="70443-236">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-236">N/A</span></span>|  
|<span data-ttu-id="70443-237">timeout</span><span class="sxs-lookup"><span data-stu-id="70443-237">timeout</span></span>|<span data-ttu-id="70443-238">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="70443-238">An integer.</span></span> <span data-ttu-id="70443-239">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="70443-239">Expression allowed.</span></span> <span data-ttu-id="70443-240">Určuje hello sekundách žádost čekat tooenter obor než selže s "403 příliš mnoho požadavků"</span><span class="sxs-lookup"><span data-stu-id="70443-240">Specifies hello number of seconds a request should wait tooenter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="70443-241">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-241">No</span></span>|<span data-ttu-id="70443-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="70443-242">Infinity</span></span>|  
|<span data-ttu-id="70443-243">Maximální délka fronty</span><span class="sxs-lookup"><span data-stu-id="70443-243">max-queue-length</span></span>|<span data-ttu-id="70443-244">Celé číslo.</span><span class="sxs-lookup"><span data-stu-id="70443-244">An integer.</span></span> <span data-ttu-id="70443-245">Výraz povoleny.</span><span class="sxs-lookup"><span data-stu-id="70443-245">Expression allowed.</span></span> <span data-ttu-id="70443-246">Určuje hello maximální délka fronty.</span><span class="sxs-lookup"><span data-stu-id="70443-246">Specifies hello maximum queue length.</span></span> <span data-ttu-id="70443-247">Příchozí požadavky při tooenter tuto zásadu bude ukončena s "403 příliš mnoho požadavků" okamžitě po vyčerpání hello fronty.</span><span class="sxs-lookup"><span data-stu-id="70443-247">Incoming requests trying tooenter this policy will be terminated with “403 Too Many Requests” immediately when hello queue is exhausted.</span></span>|<span data-ttu-id="70443-248">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-248">No</span></span>|<span data-ttu-id="70443-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="70443-249">Infinity</span></span>|  
  
###  <span data-ttu-id="70443-250"><a name="ChooseUsage"></a>Využití</span><span class="sxs-lookup"><span data-stu-id="70443-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="70443-251">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-251">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-252">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-253">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="70443-254"><a name="log-to-eventhub"></a>Protokol tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="70443-254"><a name="log-to-eventhub"></a> Log tooEvent Hub</span></span>  
 <span data-ttu-id="70443-255">Hello `log-to-eventhub` zásad odesílá zprávy v hello zadaný formát tooan centra událostí, které jsou definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="70443-255">hello `log-to-eventhub` policy sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="70443-256">Jak již název napovídá, hello zásada se používá pro uložení vybraného požadavku nebo odpovědi kontextové informace pro analýzu online nebo offline.</span><span class="sxs-lookup"><span data-stu-id="70443-256">As its name implies, hello policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="70443-257">Podrobné informace o konfiguraci centra událostí a protokolování událostí najdete v tématu [jak toolog události API Management s Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="70443-257">For a step-by-step guide on configuring an event hub and logging events, see [How toolog API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-258">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-259">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-259">Example</span></span>  
 <span data-ttu-id="70443-260">Libovolný řetězec, můžete použít jako toobe hodnota hello přihlášení služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="70443-260">Any string can be used as hello value toobe logged in Event Hubs.</span></span> <span data-ttu-id="70443-261">V tomto příkladu hello datum a čas, název služby pro nasazení, id žádosti, ip adresu a název operace pro všechny příchozí volání jsou centra událostí zaznamenané toohello protokolovacího nástroje zaregistrována hello `contoso-logger` id.</span><span class="sxs-lookup"><span data-stu-id="70443-261">In this example hello date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged toohello event hub Logger registered with hello `contoso-logger` id.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-262">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-262">Elements</span></span>  
  
|<span data-ttu-id="70443-263">Element</span><span class="sxs-lookup"><span data-stu-id="70443-263">Element</span></span>|<span data-ttu-id="70443-264">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-264">Description</span></span>|<span data-ttu-id="70443-265">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-266">protokol eventhub</span><span class="sxs-lookup"><span data-stu-id="70443-266">log-to-eventhub</span></span>|<span data-ttu-id="70443-267">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-267">Root element.</span></span> <span data-ttu-id="70443-268">Hodnota Hello tohoto elementu je centra událostí tooyour toolog řetězec hello.</span><span class="sxs-lookup"><span data-stu-id="70443-268">hello value of this element is hello string toolog tooyour event hub.</span></span>|<span data-ttu-id="70443-269">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-270">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-270">Attributes</span></span>  
  
|<span data-ttu-id="70443-271">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-271">Attribute</span></span>|<span data-ttu-id="70443-272">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-272">Description</span></span>|<span data-ttu-id="70443-273">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="70443-274">id protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="70443-274">logger-id</span></span>|<span data-ttu-id="70443-275">Hello id hello protokolovacího nástroje registrován u služby API Management.</span><span class="sxs-lookup"><span data-stu-id="70443-275">hello id of hello Logger registered with your API Management service.</span></span>|<span data-ttu-id="70443-276">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-276">Yes</span></span>|  
|<span data-ttu-id="70443-277">id oddílu</span><span class="sxs-lookup"><span data-stu-id="70443-277">partition-id</span></span>|<span data-ttu-id="70443-278">Určuje index hello hello oddílu, které jsou odesílány zprávy.</span><span class="sxs-lookup"><span data-stu-id="70443-278">Specifies hello index of hello partition where messages are sent.</span></span>|<span data-ttu-id="70443-279">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="70443-279">Optional.</span></span> <span data-ttu-id="70443-280">Tento atribut nemusí být použit, pokud `partition-key` se používá.</span><span class="sxs-lookup"><span data-stu-id="70443-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="70443-281">klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="70443-281">partition-key</span></span>|<span data-ttu-id="70443-282">Určuje hodnotu hello slouží k přiřazení k oddílu, odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="70443-282">Specifies hello value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="70443-283">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="70443-283">Optional.</span></span> <span data-ttu-id="70443-284">Tento atribut nemusí být použit, pokud `partition-id` se používá.</span><span class="sxs-lookup"><span data-stu-id="70443-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-285">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-285">Usage</span></span>  
 <span data-ttu-id="70443-286">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-286">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-287">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-288">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="70443-289"><a name="mock-response"></a>Imitované odpovědi</span><span class="sxs-lookup"><span data-stu-id="70443-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="70443-290">Hello `mock-response`, jako název hello znamená, že je použit toomock rozhraní API a operace.</span><span class="sxs-lookup"><span data-stu-id="70443-290">hello `mock-response`, as hello name implies, is used toomock APIs and operations.</span></span> <span data-ttu-id="70443-291">Se zruší provádění běžný kanál a vrátí volající toohello mocked odpovědi.</span><span class="sxs-lookup"><span data-stu-id="70443-291">It aborts normal pipeline execution and returns a mocked response toohello caller.</span></span> <span data-ttu-id="70443-292">Hello zásad se vždy pokusí tooreturn odpovědí nejvyšší přesnosti.</span><span class="sxs-lookup"><span data-stu-id="70443-292">hello policy always tries tooreturn responses of highest fidelity.</span></span> <span data-ttu-id="70443-293">Dává přednost obsahu příklady odpovědi, vždy, když je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70443-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="70443-294">Generuje ukázka odpovědí z schémata, když jsou k dispozici schémata a příklady nejsou.</span><span class="sxs-lookup"><span data-stu-id="70443-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="70443-295">Pokud ani příklady nebo schémata jsou nalezena, vrátí se odpovědi s žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="70443-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="70443-296">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="70443-297">Příklady</span><span class="sxs-lookup"><span data-stu-id="70443-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-298">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-298">Elements</span></span>  
  
|<span data-ttu-id="70443-299">Element</span><span class="sxs-lookup"><span data-stu-id="70443-299">Element</span></span>|<span data-ttu-id="70443-300">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-300">Description</span></span>|<span data-ttu-id="70443-301">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-302">model odpovědi</span><span class="sxs-lookup"><span data-stu-id="70443-302">mock-response</span></span>|<span data-ttu-id="70443-303">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-303">Root element.</span></span>|<span data-ttu-id="70443-304">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-305">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-305">Attributes</span></span>  
  
|<span data-ttu-id="70443-306">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-306">Attribute</span></span>|<span data-ttu-id="70443-307">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-307">Description</span></span>|<span data-ttu-id="70443-308">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-308">Required</span></span>|<span data-ttu-id="70443-309">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="70443-310">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="70443-310">status-code</span></span>|<span data-ttu-id="70443-311">Určuje stavový kód odpovědi a je použité tooselect odpovídající příklad nebo schéma.</span><span class="sxs-lookup"><span data-stu-id="70443-311">Specifies response status code and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="70443-312">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-312">No</span></span>|<span data-ttu-id="70443-313">200</span><span class="sxs-lookup"><span data-stu-id="70443-313">200</span></span>|  
|<span data-ttu-id="70443-314">Typ obsahu</span><span class="sxs-lookup"><span data-stu-id="70443-314">content-type</span></span>|<span data-ttu-id="70443-315">Určuje `Content-Type` hodnota hlavičky odpovědi a odpovídající příklad použité tooselect nebo schéma.</span><span class="sxs-lookup"><span data-stu-id="70443-315">Specifies `Content-Type` response header value and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="70443-316">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-316">No</span></span>|<span data-ttu-id="70443-317">Žádný</span><span class="sxs-lookup"><span data-stu-id="70443-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-318">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-318">Usage</span></span>  
 <span data-ttu-id="70443-319">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-319">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-320">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="70443-321">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="70443-322"><a name="Retry"></a>Opakování</span><span class="sxs-lookup"><span data-stu-id="70443-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="70443-323">Hello `retry` zásady provede jeho podřízených zásady jednou a pak opakuje jejich spuštění, dokud hello opakování `condition` stane `false` nebo opakujte `count` je vyčerpání.</span><span class="sxs-lookup"><span data-stu-id="70443-323">hello             `retry` policy executes its child policies once and then retries their execution until hello retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-324">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-324">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="70443-325">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-325">Example</span></span>  
 <span data-ttu-id="70443-326">V hello se pokus o následující příklad požadavek forewarding až tooten časy pomocí algoritmu exponenciální opakování.</span><span class="sxs-lookup"><span data-stu-id="70443-326">In hello following example request forewarding is retried up tooten times using exponential retry algorithm.</span></span> <span data-ttu-id="70443-327">Vzhledem k tomu `first-fast-retry` nastavena toofalse, všechny pokusy jsou subjektu toohello exponsntial opakování algoritmus.</span><span class="sxs-lookup"><span data-stu-id="70443-327">Since                    `first-fast-retry` is set toofalse, all retry attempts are subject toohello exponsntial retry algorithm.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-328">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-328">Elements</span></span>  
  
|<span data-ttu-id="70443-329">Element</span><span class="sxs-lookup"><span data-stu-id="70443-329">Element</span></span>|<span data-ttu-id="70443-330">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-330">Description</span></span>|<span data-ttu-id="70443-331">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-332">retry</span><span class="sxs-lookup"><span data-stu-id="70443-332">retry</span></span>|<span data-ttu-id="70443-333">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-333">Root element.</span></span> <span data-ttu-id="70443-334">Může obsahovat další zásady jako jeho podřízených elementů.</span><span class="sxs-lookup"><span data-stu-id="70443-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="70443-335">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-336">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-336">Attributes</span></span>  
  
|<span data-ttu-id="70443-337">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-337">Attribute</span></span>|<span data-ttu-id="70443-338">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-338">Description</span></span>|<span data-ttu-id="70443-339">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-339">Required</span></span>|<span data-ttu-id="70443-340">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-341">Podmínka</span><span class="sxs-lookup"><span data-stu-id="70443-341">condition</span></span>|<span data-ttu-id="70443-342">Logická hodnota literál nebo [výraz](api-management-policy-expressions.md) zadání, pokud by se měla zastavit opakovaných pokusů (`false`) nebo pokračování (`true`).</span><span class="sxs-lookup"><span data-stu-id="70443-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="70443-343">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-343">Yes</span></span>|<span data-ttu-id="70443-344">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-344">N/A</span></span>|  
|<span data-ttu-id="70443-345">Počet</span><span class="sxs-lookup"><span data-stu-id="70443-345">count</span></span>|<span data-ttu-id="70443-346">Kladné číslo určující maximální počet opakování tooattempt hello.</span><span class="sxs-lookup"><span data-stu-id="70443-346">A positive number specifying hello maximum number of retries tooattempt.</span></span>|<span data-ttu-id="70443-347">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-347">Yes</span></span>|<span data-ttu-id="70443-348">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-348">N/A</span></span>|  
|<span data-ttu-id="70443-349">interval</span><span class="sxs-lookup"><span data-stu-id="70443-349">interval</span></span>|<span data-ttu-id="70443-350">Kladné číslo v sekundách zadání intervalu čekání hello mezi pokusy o opakování hello.</span><span class="sxs-lookup"><span data-stu-id="70443-350">A positive number in seconds specifying hello wait interval between hello retry attempts.</span></span>|<span data-ttu-id="70443-351">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-351">Yes</span></span>|<span data-ttu-id="70443-352">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-352">N/A</span></span>|  
|<span data-ttu-id="70443-353">maximální interval</span><span class="sxs-lookup"><span data-stu-id="70443-353">max-interval</span></span>|<span data-ttu-id="70443-354">Kladné číslo v sekundách zadání hello maximální čekání interval mezi pokusy o opakování hello.</span><span class="sxs-lookup"><span data-stu-id="70443-354">A positive number in seconds specifying hello maximum wait interval between hello retry attempts.</span></span> <span data-ttu-id="70443-355">Je použité tooimplement algoritmu exponenciální opakování.</span><span class="sxs-lookup"><span data-stu-id="70443-355">It is used tooimplement an exponential retry algorithm.</span></span>|<span data-ttu-id="70443-356">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-356">No</span></span>|<span data-ttu-id="70443-357">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-357">N/A</span></span>|  
|<span data-ttu-id="70443-358">rozdílů</span><span class="sxs-lookup"><span data-stu-id="70443-358">delta</span></span>|<span data-ttu-id="70443-359">Kladné číslo v sekundách zadání přírůstek intervalu čekání hello.</span><span class="sxs-lookup"><span data-stu-id="70443-359">A positive number in seconds specifying hello wait interval increment.</span></span> <span data-ttu-id="70443-360">Je použité tooimplement hello lineární a exponenciální opakování algoritmy.</span><span class="sxs-lookup"><span data-stu-id="70443-360">It is used tooimplement hello linear and exponential retry algorithms.</span></span>|<span data-ttu-id="70443-361">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-361">No</span></span>|<span data-ttu-id="70443-362">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-362">N/A</span></span>|  
|<span data-ttu-id="70443-363">Zkuste zopakovat první fast</span><span class="sxs-lookup"><span data-stu-id="70443-363">first-fast-retry</span></span>|<span data-ttu-id="70443-364">Pokud nastavení příliš `true` , hello první pokus o opakování provádí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="70443-364">If set too                                   `true` , hello first retry attempt is performed immediately.</span></span>|<span data-ttu-id="70443-365">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="70443-366">Když pouze hello `interval` není zadaný, **pevné** jsou prováděny interval opakování.</span><span class="sxs-lookup"><span data-stu-id="70443-366">When only hello `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="70443-367">Když pouze hello `interval` a `delta` nejsou zadány, **lineární** se použije algoritmus interval opakování, kde je doba čekání mezi opakovanými pokusy počítané podle hello následující vzorec - `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="70443-367">When only hello `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according hello following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="70443-368">Když hello `interval`, `max-interval` a `delta` jsou nastaveny, **exponenciální** interval opakování algoritmus se použije, kde je doba čekání hello mezi opakovanými pokusy hello zvětšování exponenciálně od hodnoty hello `interval`toohello hodnotu `max-interval` podle následující toohello forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="70443-368">When hello `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where hello wait time between hello retries is growing exponentially from hello value of `interval` toohello value `max-interval` according toohello following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="70443-369">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-369">Usage</span></span>  
 <span data-ttu-id="70443-370">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="70443-370">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="70443-371">Všimněte si, že podřízená omezení použití zásad zdědí tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="70443-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="70443-372">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-373">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-374"><a name="ReturnResponse"></a>Vrátí odpověď</span><span class="sxs-lookup"><span data-stu-id="70443-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="70443-375">Hello `return-response` zásady zruší provádění zřetězeného příkazu a vrací výchozí nebo vlastní odpovědi toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="70443-375">hello `return-response` policy aborts pipeline execution and returns either a default or custom response toohello caller.</span></span> <span data-ttu-id="70443-376">Výchozí odpověď je `200 OK` se žádné textem.</span><span class="sxs-lookup"><span data-stu-id="70443-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="70443-377">Vlastní odpovědi lze zadat pomocí kontextu proměnné nebo zásady příkazy.</span><span class="sxs-lookup"><span data-stu-id="70443-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="70443-378">Pokud obě jsou k dispozici, hello odpovědi obsažené v hello kontextové proměnné je upravit příkazy zásad hello před vrácením toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="70443-378">When both are provided, hello response contained within hello context variable is modified by hello policy statements before being returned toohello caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-379">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-380">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-381">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-381">Elements</span></span>  
  
|<span data-ttu-id="70443-382">Element</span><span class="sxs-lookup"><span data-stu-id="70443-382">Element</span></span>|<span data-ttu-id="70443-383">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-383">Description</span></span>|<span data-ttu-id="70443-384">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-385">vrátí odpověď</span><span class="sxs-lookup"><span data-stu-id="70443-385">return-response</span></span>|<span data-ttu-id="70443-386">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-386">Root element.</span></span>|<span data-ttu-id="70443-387">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-387">Yes</span></span>|  
|<span data-ttu-id="70443-388">set – hlavička</span><span class="sxs-lookup"><span data-stu-id="70443-388">set-header</span></span>|<span data-ttu-id="70443-389">A [sadu hlaviček](api-management-transformation-policies.md#SetHTTPheader) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="70443-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="70443-390">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-390">No</span></span>|  
|<span data-ttu-id="70443-391">Sada textu</span><span class="sxs-lookup"><span data-stu-id="70443-391">set-body</span></span>|<span data-ttu-id="70443-392">A [set textu](api-management-transformation-policies.md#SetBody) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="70443-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="70443-393">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-393">No</span></span>|  
|<span data-ttu-id="70443-394">Nastavit stav</span><span class="sxs-lookup"><span data-stu-id="70443-394">set-status</span></span>|<span data-ttu-id="70443-395">A [nastavit stav](api-management-advanced-policies.md#SetStatus) prohlášení o zásadách.</span><span class="sxs-lookup"><span data-stu-id="70443-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="70443-396">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-397">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-397">Attributes</span></span>  
  
|<span data-ttu-id="70443-398">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-398">Attribute</span></span>|<span data-ttu-id="70443-399">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-399">Description</span></span>|<span data-ttu-id="70443-400">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="70443-401">Název proměnné odpovědi</span><span class="sxs-lookup"><span data-stu-id="70443-401">response-variable-name</span></span>|<span data-ttu-id="70443-402">Hello název kontextové proměnné hello na něj odkazovat z, například předcházejícího [odeslán požadavek](api-management-advanced-policies.md#SendRequest) zásady a obsahující `Response` objektu</span><span class="sxs-lookup"><span data-stu-id="70443-402">hello name of hello context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="70443-403">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="70443-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-404">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-404">Usage</span></span>  
 <span data-ttu-id="70443-405">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-405">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-406">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-407">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-408"><a name="SendOneWayRequest"></a>Odeslání žádosti o jednorázové jednoduché</span><span class="sxs-lookup"><span data-stu-id="70443-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="70443-409">Hello `send-one-way-request` zásad odešle požadavek hello poskytuje toohello zadané adresy URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="70443-409">hello `send-one-way-request` policy sends hello provided request toohello specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-410">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-411">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-411">Example</span></span>  
 <span data-ttu-id="70443-412">Tato ukázková zásada ukazuje příklad použití hello `send-one-way-request` toosend zásad v zpráva tooa Slack chatovací místnosti Pokud hello kódu odpovědi HTTP je větší než nebo rovna too500.</span><span class="sxs-lookup"><span data-stu-id="70443-412">This sample policy shows an example of using hello `send-one-way-request` policy toosend a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="70443-413">Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="70443-413">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-414">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-414">Elements</span></span>  
  
|<span data-ttu-id="70443-415">Element</span><span class="sxs-lookup"><span data-stu-id="70443-415">Element</span></span>|<span data-ttu-id="70443-416">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-416">Description</span></span>|<span data-ttu-id="70443-417">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-418">odeslání jeden způsob požadavků</span><span class="sxs-lookup"><span data-stu-id="70443-418">send-one-way-request</span></span>|<span data-ttu-id="70443-419">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-419">Root element.</span></span>|<span data-ttu-id="70443-420">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-420">Yes</span></span>|  
|<span data-ttu-id="70443-421">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="70443-421">url</span></span>|<span data-ttu-id="70443-422">Adresa URL Hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-422">hello URL of hello request.</span></span>|<span data-ttu-id="70443-423">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="70443-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="70443-424">– Metoda</span><span class="sxs-lookup"><span data-stu-id="70443-424">method</span></span>|<span data-ttu-id="70443-425">Metoda HTTP pro žádost o hello Hello.</span><span class="sxs-lookup"><span data-stu-id="70443-425">hello HTTP method for hello request.</span></span>|<span data-ttu-id="70443-426">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="70443-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="70443-427">záhlaví</span><span class="sxs-lookup"><span data-stu-id="70443-427">header</span></span>|<span data-ttu-id="70443-428">Hlavička požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-428">Request header.</span></span> <span data-ttu-id="70443-429">Použijte více prvky záhlaví pro více hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="70443-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="70443-430">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-430">No</span></span>|  
|<span data-ttu-id="70443-431">Text</span><span class="sxs-lookup"><span data-stu-id="70443-431">body</span></span>|<span data-ttu-id="70443-432">datová část požadavku Hello.</span><span class="sxs-lookup"><span data-stu-id="70443-432">hello request body.</span></span>|<span data-ttu-id="70443-433">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-434">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-434">Attributes</span></span>  
  
|<span data-ttu-id="70443-435">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-435">Attribute</span></span>|<span data-ttu-id="70443-436">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-436">Description</span></span>|<span data-ttu-id="70443-437">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-437">Required</span></span>|<span data-ttu-id="70443-438">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-439">režim = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-439">mode="string"</span></span>|<span data-ttu-id="70443-440">Určuje, jestli je nový požadavek nebo kopii hello aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="70443-440">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="70443-441">V odchozí režim režim = kopie neinicializuje hello textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-441">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="70443-442">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-442">No</span></span>|<span data-ttu-id="70443-443">Nový</span><span class="sxs-lookup"><span data-stu-id="70443-443">New</span></span>|  
|<span data-ttu-id="70443-444">jméno</span><span class="sxs-lookup"><span data-stu-id="70443-444">name</span></span>|<span data-ttu-id="70443-445">Určuje název hello hello záhlaví toobe sady.</span><span class="sxs-lookup"><span data-stu-id="70443-445">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="70443-446">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-446">Yes</span></span>|<span data-ttu-id="70443-447">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-447">N/A</span></span>|  
|<span data-ttu-id="70443-448">existuje akce</span><span class="sxs-lookup"><span data-stu-id="70443-448">exists-action</span></span>|<span data-ttu-id="70443-449">Určuje jaké akce tootake při hello záhlaví byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="70443-449">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="70443-450">Tento atribut musí mít jednu z následujících hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="70443-450">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="70443-451">-Přepište - hodnotu hello nahradí existující záhlaví hello.</span><span class="sxs-lookup"><span data-stu-id="70443-451">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="70443-452">-skip - nenahrazuje hello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="70443-452">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="70443-453">-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="70443-453">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="70443-454">-delete - odebere hello hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="70443-454">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="70443-455">Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="70443-455">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="70443-456">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-456">No</span></span>|<span data-ttu-id="70443-457">přepsání</span><span class="sxs-lookup"><span data-stu-id="70443-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-458">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-458">Usage</span></span>  
 <span data-ttu-id="70443-459">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-459">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-460">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-461">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-462"><a name="SendRequest"></a>Odeslání požadavku</span><span class="sxs-lookup"><span data-stu-id="70443-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="70443-463">Hello `send-request` zásad odešle požadavek hello poskytuje toohello zadat adresu URL, už čeká, než hello nastavit hodnotu časového limitu.</span><span class="sxs-lookup"><span data-stu-id="70443-463">hello `send-request` policy sends hello provided request toohello specified URL, waiting no longer than hello set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-464">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-465">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-465">Example</span></span>  
 <span data-ttu-id="70443-466">Tento příklad ukazuje jeden ze způsobů tooverify token odkazu se serverem ověřování.</span><span class="sxs-lookup"><span data-stu-id="70443-466">This example shows one way tooverify a reference token with an authorization server.</span></span> <span data-ttu-id="70443-467">Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="70443-467">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request tooToken Server toovalidate token (see RFC 7662) -->  
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
  
### <a name="elements"></a><span data-ttu-id="70443-468">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-468">Elements</span></span>  
  
|<span data-ttu-id="70443-469">Element</span><span class="sxs-lookup"><span data-stu-id="70443-469">Element</span></span>|<span data-ttu-id="70443-470">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-470">Description</span></span>|<span data-ttu-id="70443-471">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-472">odeslání žádosti</span><span class="sxs-lookup"><span data-stu-id="70443-472">send-request</span></span>|<span data-ttu-id="70443-473">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-473">Root element.</span></span>|<span data-ttu-id="70443-474">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-474">Yes</span></span>|  
|<span data-ttu-id="70443-475">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="70443-475">url</span></span>|<span data-ttu-id="70443-476">Adresa URL Hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-476">hello URL of hello request.</span></span>|<span data-ttu-id="70443-477">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="70443-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="70443-478">– Metoda</span><span class="sxs-lookup"><span data-stu-id="70443-478">method</span></span>|<span data-ttu-id="70443-479">Metoda HTTP pro žádost o hello Hello.</span><span class="sxs-lookup"><span data-stu-id="70443-479">hello HTTP method for hello request.</span></span>|<span data-ttu-id="70443-480">Pokud žádné režimu = kopie; v opačném případě Ano.</span><span class="sxs-lookup"><span data-stu-id="70443-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="70443-481">záhlaví</span><span class="sxs-lookup"><span data-stu-id="70443-481">header</span></span>|<span data-ttu-id="70443-482">Hlavička požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-482">Request header.</span></span> <span data-ttu-id="70443-483">Použijte více prvky záhlaví pro více hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="70443-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="70443-484">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-484">No</span></span>|  
|<span data-ttu-id="70443-485">Text</span><span class="sxs-lookup"><span data-stu-id="70443-485">body</span></span>|<span data-ttu-id="70443-486">datová část požadavku Hello.</span><span class="sxs-lookup"><span data-stu-id="70443-486">hello request body.</span></span>|<span data-ttu-id="70443-487">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-488">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-488">Attributes</span></span>  
  
|<span data-ttu-id="70443-489">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-489">Attribute</span></span>|<span data-ttu-id="70443-490">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-490">Description</span></span>|<span data-ttu-id="70443-491">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-491">Required</span></span>|<span data-ttu-id="70443-492">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-493">režim = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-493">mode="string"</span></span>|<span data-ttu-id="70443-494">Určuje, jestli je nový požadavek nebo kopii hello aktuální požadavek.</span><span class="sxs-lookup"><span data-stu-id="70443-494">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="70443-495">V odchozí režim režim = kopie neinicializuje hello textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="70443-495">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="70443-496">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-496">No</span></span>|<span data-ttu-id="70443-497">Nový</span><span class="sxs-lookup"><span data-stu-id="70443-497">New</span></span>|  
|<span data-ttu-id="70443-498">Název proměnné odpovědi = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-498">response-variable-name="string"</span></span>|<span data-ttu-id="70443-499">Pokud není přítomný, `context.Response` se používá.</span><span class="sxs-lookup"><span data-stu-id="70443-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="70443-500">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-500">No</span></span>|<span data-ttu-id="70443-501">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-501">N/A</span></span>|  
|<span data-ttu-id="70443-502">časový limit = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="70443-502">timeout="integer"</span></span>|<span data-ttu-id="70443-503">Hello interval časového limitu v sekundách, než hello volání toohello adresa URL se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="70443-503">hello timeout interval in seconds before hello call toohello URL fails.</span></span>|<span data-ttu-id="70443-504">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-504">No</span></span>|<span data-ttu-id="70443-505">60</span><span class="sxs-lookup"><span data-stu-id="70443-505">60</span></span>|  
|<span data-ttu-id="70443-506">Ignorovat chybu</span><span class="sxs-lookup"><span data-stu-id="70443-506">ignore-error</span></span>|<span data-ttu-id="70443-507">Pokud hodnotu true a hello požadavku dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="70443-507">If true and hello request results in an error:</span></span><br /><br /> <span data-ttu-id="70443-508">– Pokud je název proměnné odpověď byla zadána, bude obsahovat hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="70443-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="70443-509">– Pokud nebyl zadán název proměnné odpovědi, kontextu. Požadavek nebude aktualizován.</span><span class="sxs-lookup"><span data-stu-id="70443-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="70443-510">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-510">No</span></span>|<span data-ttu-id="70443-511">False</span><span class="sxs-lookup"><span data-stu-id="70443-511">false</span></span>|  
|<span data-ttu-id="70443-512">jméno</span><span class="sxs-lookup"><span data-stu-id="70443-512">name</span></span>|<span data-ttu-id="70443-513">Určuje název hello hello záhlaví toobe sady.</span><span class="sxs-lookup"><span data-stu-id="70443-513">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="70443-514">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-514">Yes</span></span>|<span data-ttu-id="70443-515">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-515">N/A</span></span>|  
|<span data-ttu-id="70443-516">existuje akce</span><span class="sxs-lookup"><span data-stu-id="70443-516">exists-action</span></span>|<span data-ttu-id="70443-517">Určuje jaké akce tootake při hello záhlaví byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="70443-517">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="70443-518">Tento atribut musí mít jednu z následujících hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="70443-518">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="70443-519">-Přepište - hodnotu hello nahradí existující záhlaví hello.</span><span class="sxs-lookup"><span data-stu-id="70443-519">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="70443-520">-skip - nenahrazuje hello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="70443-520">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="70443-521">-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="70443-521">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="70443-522">-delete - odebere hello hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="70443-522">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="70443-523">Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="70443-523">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="70443-524">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-524">No</span></span>|<span data-ttu-id="70443-525">přepsání</span><span class="sxs-lookup"><span data-stu-id="70443-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-526">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-526">Usage</span></span>  
 <span data-ttu-id="70443-527">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-527">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-528">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-529">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-530"><a name="SetHttpProxy"></a>Server proxy protokolu HTTP sady</span><span class="sxs-lookup"><span data-stu-id="70443-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="70443-531">Hello `proxy` zásad vám umožní tooroute požadavky předané toobackends prostřednictvím proxy serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="70443-531">hello `proxy` policy allows you tooroute requests forwarded toobackends via an HTTP proxy.</span></span> <span data-ttu-id="70443-532">Mezi hello brány a hello proxy je podporována jen protokol HTTP (nikoli HTTPS).</span><span class="sxs-lookup"><span data-stu-id="70443-532">Only HTTP (not HTTPS) is supported between hello gateway and hello proxy.</span></span> <span data-ttu-id="70443-533">Pouze ověřování NTLM a Basic.</span><span class="sxs-lookup"><span data-stu-id="70443-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="70443-534">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-535">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-535">Example</span></span>  
<span data-ttu-id="70443-536">Všimněte si použití hello [vlastnosti](api-management-howto-properties.md) jako hodnoty hello uživatelské jméno a heslo tooavoid ukládání citlivých informací v dokumentu zásad hello.</span><span class="sxs-lookup"><span data-stu-id="70443-536">Note hello use of [properties](api-management-howto-properties.md) as values of hello username and password tooavoid storing sensitive information in hello policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-537">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-537">Elements</span></span>  
  
|<span data-ttu-id="70443-538">Element</span><span class="sxs-lookup"><span data-stu-id="70443-538">Element</span></span>|<span data-ttu-id="70443-539">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-539">Description</span></span>|<span data-ttu-id="70443-540">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-541">Proxy server</span><span class="sxs-lookup"><span data-stu-id="70443-541">proxy</span></span>|<span data-ttu-id="70443-542">Kořenový element</span><span class="sxs-lookup"><span data-stu-id="70443-542">Root element</span></span>|<span data-ttu-id="70443-543">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="70443-544">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-544">Attributes</span></span>  
  
|<span data-ttu-id="70443-545">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-545">Attribute</span></span>|<span data-ttu-id="70443-546">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-546">Description</span></span>|<span data-ttu-id="70443-547">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-547">Required</span></span>|<span data-ttu-id="70443-548">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-549">Adresa URL = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-549">url="string"</span></span>|<span data-ttu-id="70443-550">Adresa URL proxy serveru v hello formu http://host:port.</span><span class="sxs-lookup"><span data-stu-id="70443-550">Proxy URL in hello form of http://host:port.</span></span>|<span data-ttu-id="70443-551">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-551">Yes</span></span>|<span data-ttu-id="70443-552">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-552">N/A</span></span>|  
|<span data-ttu-id="70443-553">uživatelské jméno = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-553">username="string"</span></span>|<span data-ttu-id="70443-554">Uživatelské jméno toobe používá k ověřování připojení k proxy serveru hello.</span><span class="sxs-lookup"><span data-stu-id="70443-554">Username toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="70443-555">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-555">No</span></span>|<span data-ttu-id="70443-556">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-556">N/A</span></span>|  
|<span data-ttu-id="70443-557">heslo = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-557">password="string"</span></span>|<span data-ttu-id="70443-558">Toobe heslo použít k ověřování připojení k proxy serveru hello.</span><span class="sxs-lookup"><span data-stu-id="70443-558">Password toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="70443-559">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-559">No</span></span>|<span data-ttu-id="70443-560">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="70443-561">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-561">Usage</span></span>  
 <span data-ttu-id="70443-562">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-562">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-563">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="70443-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="70443-564">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="70443-565"><a name="SetRequestMethod"></a>Žádost o set – metoda</span><span class="sxs-lookup"><span data-stu-id="70443-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="70443-566">Hello `set-method` zásad vám umožní metoda žádosti toochange hello HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="70443-566">hello `set-method` policy allows you toochange hello HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-567">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-568">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-568">Example</span></span>  
 <span data-ttu-id="70443-569">Tato ukázka zásadu, která používá hello `set-method` zásad ukazuje příklad odesílání Slack chatovací místnosti tooa zprávu, pokud hello kódu odpovědi HTTP je větší než nebo rovna too500.</span><span class="sxs-lookup"><span data-stu-id="70443-569">This sample policy that uses hello `set-method` policy shows an example of sending a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="70443-570">Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="70443-570">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-571">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-571">Elements</span></span>  
  
|<span data-ttu-id="70443-572">Element</span><span class="sxs-lookup"><span data-stu-id="70443-572">Element</span></span>|<span data-ttu-id="70443-573">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-573">Description</span></span>|<span data-ttu-id="70443-574">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-575">set – metoda</span><span class="sxs-lookup"><span data-stu-id="70443-575">set-method</span></span>|<span data-ttu-id="70443-576">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-576">Root element.</span></span> <span data-ttu-id="70443-577">Hodnota Hello elementu hello určuje metoda HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="70443-577">hello value of hello element specifies hello HTTP method.</span></span>|<span data-ttu-id="70443-578">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-579">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-579">Usage</span></span>  
 <span data-ttu-id="70443-580">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-580">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-581">**Části zásady:** příchozí, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="70443-582">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-583"><a name="SetStatus"></a>Sada stavový kód</span><span class="sxs-lookup"><span data-stu-id="70443-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="70443-584">Hello `set-status` zásad sady hello HTTP stavový kód toohello zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="70443-584">hello `set-status` policy sets hello HTTP status code toohello specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-585">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-586">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-586">Example</span></span>  
 <span data-ttu-id="70443-587">Tento příklad ukazuje, jak tooreturn odpovědi 401, pokud hello autorizační token je neplatný.</span><span class="sxs-lookup"><span data-stu-id="70443-587">This example shows how tooreturn a 401 response if hello authorization token is invalid.</span></span> <span data-ttu-id="70443-588">Další informace najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="70443-588">For more information, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-589">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-589">Elements</span></span>  
  
|<span data-ttu-id="70443-590">Element</span><span class="sxs-lookup"><span data-stu-id="70443-590">Element</span></span>|<span data-ttu-id="70443-591">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-591">Description</span></span>|<span data-ttu-id="70443-592">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-593">Nastavit stav</span><span class="sxs-lookup"><span data-stu-id="70443-593">set-status</span></span>|<span data-ttu-id="70443-594">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-594">Root element.</span></span>|<span data-ttu-id="70443-595">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-596">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-596">Attributes</span></span>  
  
|<span data-ttu-id="70443-597">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-597">Attribute</span></span>|<span data-ttu-id="70443-598">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-598">Description</span></span>|<span data-ttu-id="70443-599">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-599">Required</span></span>|<span data-ttu-id="70443-600">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-601">kód = "celé číslo"</span><span class="sxs-lookup"><span data-stu-id="70443-601">code="integer"</span></span>|<span data-ttu-id="70443-602">tooreturn kód stavu Hello HTTP.</span><span class="sxs-lookup"><span data-stu-id="70443-602">hello HTTP status code tooreturn.</span></span>|<span data-ttu-id="70443-603">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-603">Yes</span></span>|<span data-ttu-id="70443-604">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-604">N/A</span></span>|  
|<span data-ttu-id="70443-605">důvod = "řetězec"</span><span class="sxs-lookup"><span data-stu-id="70443-605">reason="string"</span></span>|<span data-ttu-id="70443-606">Popis hello důvod pro vrácení hello stavový kód.</span><span class="sxs-lookup"><span data-stu-id="70443-606">A description of hello reason for returning hello status code.</span></span>|<span data-ttu-id="70443-607">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-607">Yes</span></span>|<span data-ttu-id="70443-608">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-609">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-609">Usage</span></span>  
 <span data-ttu-id="70443-610">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-610">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-611">**Části zásady:** odchozí, back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-612">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="70443-613"><a name="set-variable"></a>Nastavená proměnná</span><span class="sxs-lookup"><span data-stu-id="70443-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="70443-614">Hello `set-variable` deklaruje zásad [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné a přiřadí ji má hodnotu prostřednictvím [výraz](api-management-policy-expressions.md) nebo řetězcový literál.</span><span class="sxs-lookup"><span data-stu-id="70443-614">hello `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="70443-615">Pokud hello výraz obsahuje literál se převede tooa řetězec a hello typ hodnoty hello bude `System.String`.</span><span class="sxs-lookup"><span data-stu-id="70443-615">if hello expression contains a literal it will be converted tooa string and hello type of hello value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="70443-616"><a name="set-variablePolicyStatement"></a>Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="70443-617"><a name="set-variableExample"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="70443-618">Hello následující příklad ukazuje nastavení proměnné zásad v hello příchozí části.</span><span class="sxs-lookup"><span data-stu-id="70443-618">hello following example demonstrates a set variable policy in hello inbound section.</span></span> <span data-ttu-id="70443-619">Toto nastavení zásad proměnné vytvoří `isMobile` Boolean [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena tootrue, pokud hello `User-Agent` žádosti záhlaví obsahuje hello text `iPad` nebo `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="70443-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-620">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-620">Elements</span></span>  
  
|<span data-ttu-id="70443-621">Element</span><span class="sxs-lookup"><span data-stu-id="70443-621">Element</span></span>|<span data-ttu-id="70443-622">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-622">Description</span></span>|<span data-ttu-id="70443-623">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-624">nastavená proměnná</span><span class="sxs-lookup"><span data-stu-id="70443-624">set-variable</span></span>|<span data-ttu-id="70443-625">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-625">Root element.</span></span>|<span data-ttu-id="70443-626">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-627">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-627">Attributes</span></span>  
  
|<span data-ttu-id="70443-628">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-628">Attribute</span></span>|<span data-ttu-id="70443-629">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-629">Description</span></span>|<span data-ttu-id="70443-630">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="70443-631">jméno</span><span class="sxs-lookup"><span data-stu-id="70443-631">name</span></span>|<span data-ttu-id="70443-632">Hello název proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="70443-632">hello name of hello variable.</span></span>|<span data-ttu-id="70443-633">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-633">Yes</span></span>|  
|<span data-ttu-id="70443-634">hodnota</span><span class="sxs-lookup"><span data-stu-id="70443-634">value</span></span>|<span data-ttu-id="70443-635">Hodnota Hello hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="70443-635">hello value of hello variable.</span></span> <span data-ttu-id="70443-636">To může být výraz nebo hodnota literálu.</span><span class="sxs-lookup"><span data-stu-id="70443-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="70443-637">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-638">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-638">Usage</span></span>  
 <span data-ttu-id="70443-639">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-639">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-640">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-641">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="70443-642"><a name="set-variableAllowedTypes"></a>Povolené typy</span><span class="sxs-lookup"><span data-stu-id="70443-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="70443-643">Výrazy použité v hello `set-variable` zásady musí vrátit jednu z následujících základních typů hello.</span><span class="sxs-lookup"><span data-stu-id="70443-643">Expressions used in hello `set-variable` policy must return one of hello following basic types.</span></span>  
  
-   <span data-ttu-id="70443-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="70443-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="70443-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="70443-645">System.SByte</span></span>  
  
-   <span data-ttu-id="70443-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="70443-646">System.Byte</span></span>  
  
-   <span data-ttu-id="70443-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="70443-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="70443-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="70443-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="70443-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="70443-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="70443-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="70443-650">System.Int16</span></span>  
  
-   <span data-ttu-id="70443-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="70443-651">System.Int32</span></span>  
  
-   <span data-ttu-id="70443-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="70443-652">System.Int64</span></span>  
  
-   <span data-ttu-id="70443-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="70443-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="70443-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="70443-654">System.Single</span></span>  
  
-   <span data-ttu-id="70443-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="70443-655">System.Double</span></span>  
  
-   <span data-ttu-id="70443-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="70443-656">System.Guid</span></span>  
  
-   <span data-ttu-id="70443-657">System.String</span><span class="sxs-lookup"><span data-stu-id="70443-657">System.String</span></span>  
  
-   <span data-ttu-id="70443-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="70443-658">System.Char</span></span>  
  
-   <span data-ttu-id="70443-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="70443-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="70443-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="70443-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="70443-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="70443-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="70443-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="70443-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="70443-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="70443-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="70443-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="70443-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="70443-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="70443-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="70443-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="70443-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="70443-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="70443-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="70443-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="70443-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="70443-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="70443-669">System.Single?</span></span>  
  
-   <span data-ttu-id="70443-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="70443-670">System.Double?</span></span>  
  
-   <span data-ttu-id="70443-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="70443-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="70443-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="70443-672">System.String?</span></span>  
  
-   <span data-ttu-id="70443-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="70443-673">System.Char?</span></span>  
  
-   <span data-ttu-id="70443-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="70443-674">System.DateTime?</span></span>  

##  <span data-ttu-id="70443-675"><a name="Trace"></a>Trasování</span><span class="sxs-lookup"><span data-stu-id="70443-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="70443-676">Hello `trace` zásad přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="70443-676">hello             `trace` policy adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="70443-677">Hello zásady budou spuštěny pouze při trasování je aktivaci, tj. `Ocp-Apim-Trace` hlavička požadavku je přítomen a nastavte příliš`true` a `Ocp-Apim-Subscription-Key` hlavička požadavku je k dispozici a obsahuje platný klíč přidružený k účtu správce hello.</span><span class="sxs-lookup"><span data-stu-id="70443-677">hello policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set too`true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with hello admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-678">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="70443-679">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-679">Elements</span></span>  
  
|<span data-ttu-id="70443-680">Element</span><span class="sxs-lookup"><span data-stu-id="70443-680">Element</span></span>|<span data-ttu-id="70443-681">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-681">Description</span></span>|<span data-ttu-id="70443-682">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-683">trasování</span><span class="sxs-lookup"><span data-stu-id="70443-683">trace</span></span>|<span data-ttu-id="70443-684">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-684">Root element.</span></span>|<span data-ttu-id="70443-685">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-686">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-686">Attributes</span></span>  
  
|<span data-ttu-id="70443-687">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-687">Attribute</span></span>|<span data-ttu-id="70443-688">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-688">Description</span></span>|<span data-ttu-id="70443-689">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-689">Required</span></span>|<span data-ttu-id="70443-690">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-691">Zdroj</span><span class="sxs-lookup"><span data-stu-id="70443-691">source</span></span>|<span data-ttu-id="70443-692">Řetězcový literál smysluplný toohello trasování prohlížeč a zadání hello zdroj zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="70443-692">String literal meaningful toohello trace viewer and specifying hello source of hello message.</span></span>|<span data-ttu-id="70443-693">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-693">Yes</span></span>|<span data-ttu-id="70443-694">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="70443-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-695">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-695">Usage</span></span>  
 <span data-ttu-id="70443-696">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="70443-696">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="70443-697">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="70443-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="70443-698">**Zásady obory:** všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="70443-699"><a name="Wait"></a>Počkej</span><span class="sxs-lookup"><span data-stu-id="70443-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="70443-700">Hello `wait` zásady spustí své zásady bezprostředně podřízené paralelně a čeká všechny nebo jednoho z jeho toocomplete zásady bezprostředně podřízené před dokončením.</span><span class="sxs-lookup"><span data-stu-id="70443-700">hello `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies toocomplete before it completes.</span></span> <span data-ttu-id="70443-701">Hello čekání zásad může mít jako jeho bezprostředně podřízené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), a [řízení toku](api-management-advanced-policies.md#choose) zásady.</span><span class="sxs-lookup"><span data-stu-id="70443-701">hello wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="70443-702">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="70443-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="70443-703">Příklad</span><span class="sxs-lookup"><span data-stu-id="70443-703">Example</span></span>  
 <span data-ttu-id="70443-704">V následující ukázka hello jsou uvedeny dvě `choose` zásady jako bezprostředně podřízené zásady hello `wait` zásad.</span><span class="sxs-lookup"><span data-stu-id="70443-704">In hello following example there are two `choose` policies as immediate child policies of hello `wait` policy.</span></span> <span data-ttu-id="70443-705">Každá z těchto `choose` zásady spustí paralelně.</span><span class="sxs-lookup"><span data-stu-id="70443-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="70443-706">Každý `choose` zásad pokusí tooretrieve hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="70443-706">Each `choose` policy attempts tooretrieve a cached value.</span></span> <span data-ttu-id="70443-707">Pokud dojde k neúspěšnému přístupu do mezipaměti, se nazývá back-end službu tooprovide hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="70443-707">If there is a cache miss, a backend service is called tooprovide hello value.</span></span> <span data-ttu-id="70443-708">V tento příklad hello `wait` zásad dokončena, dokud se všechny jeho podřízené okamžitou zásady dokončit, protože hello `for` atribut je nastaven příliš`all`.</span><span class="sxs-lookup"><span data-stu-id="70443-708">In this example hello `wait` policy does not complete until all of its immediate child policies complete, because hello `for` attribute is set too`all`.</span></span>   <span data-ttu-id="70443-709">V této proměnné kontextu hello příklad (`execute-branch-one`, `value-one`, `execute-branch-two`, a `value-two`) jsou mimo rozsah hello tento příklad zásady deklarován.</span><span class="sxs-lookup"><span data-stu-id="70443-709">In this example hello context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of hello scope of this example policy.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="70443-710">Elementy</span><span class="sxs-lookup"><span data-stu-id="70443-710">Elements</span></span>  
  
|<span data-ttu-id="70443-711">Element</span><span class="sxs-lookup"><span data-stu-id="70443-711">Element</span></span>|<span data-ttu-id="70443-712">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-712">Description</span></span>|<span data-ttu-id="70443-713">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="70443-714">Počkej</span><span class="sxs-lookup"><span data-stu-id="70443-714">wait</span></span>|<span data-ttu-id="70443-715">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="70443-715">Root element.</span></span> <span data-ttu-id="70443-716">Může obsahovat pouze podřízené elementy `send-request`, `cache-lookup-value`, a `choose` zásady.</span><span class="sxs-lookup"><span data-stu-id="70443-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="70443-717">Ano</span><span class="sxs-lookup"><span data-stu-id="70443-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="70443-718">Atributy</span><span class="sxs-lookup"><span data-stu-id="70443-718">Attributes</span></span>  
  
|<span data-ttu-id="70443-719">Atribut</span><span class="sxs-lookup"><span data-stu-id="70443-719">Attribute</span></span>|<span data-ttu-id="70443-720">Popis</span><span class="sxs-lookup"><span data-stu-id="70443-720">Description</span></span>|<span data-ttu-id="70443-721">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="70443-721">Required</span></span>|<span data-ttu-id="70443-722">Výchozí</span><span class="sxs-lookup"><span data-stu-id="70443-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="70443-723">Pro</span><span class="sxs-lookup"><span data-stu-id="70443-723">for</span></span>|<span data-ttu-id="70443-724">Určuje, zda hello `wait` zásad čeká na všechny zásady toobe bezprostředně podřízené dokončit nebo jenom jeden.</span><span class="sxs-lookup"><span data-stu-id="70443-724">Determines whether hello `wait` policy waits for all immediate child policies toobe completed or just one.</span></span> <span data-ttu-id="70443-725">Povolené hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="70443-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="70443-726">-   `all`-čekat na všechny zásady toocomplete bezprostředně podřízené</span><span class="sxs-lookup"><span data-stu-id="70443-726">-   `all` - wait for all immediate child policies toocomplete</span></span><br /><span data-ttu-id="70443-727">-všechny - počkejte žádné zásady toocomplete bezprostředně podřízené.</span><span class="sxs-lookup"><span data-stu-id="70443-727">-   any - wait for any immediate child policy toocomplete.</span></span> <span data-ttu-id="70443-728">Po dokončení hello první bezprostředně podřízené zásad hello `wait` zásad dokončí a provádění dalších zásad bezprostředně podřízené je ukončen.</span><span class="sxs-lookup"><span data-stu-id="70443-728">Once hello first immediate child policy has completed, hello `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="70443-729">Ne</span><span class="sxs-lookup"><span data-stu-id="70443-729">No</span></span>|<span data-ttu-id="70443-730">Všechny</span><span class="sxs-lookup"><span data-stu-id="70443-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="70443-731">Využití</span><span class="sxs-lookup"><span data-stu-id="70443-731">Usage</span></span>  
 <span data-ttu-id="70443-732">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="70443-732">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="70443-733">**Části zásady:** vstupní, výstupní a back-end</span><span class="sxs-lookup"><span data-stu-id="70443-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="70443-734">**Zásady obory:**všechny obory</span><span class="sxs-lookup"><span data-stu-id="70443-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="70443-735">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70443-735">Next steps</span></span>
<span data-ttu-id="70443-736">Práce se zásadami pro další informace najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="70443-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="70443-737">Zásady ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="70443-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="70443-738">Výrazy zásad</span><span class="sxs-lookup"><span data-stu-id="70443-738">Policy expressions</span></span>](api-management-policy-expressions.md)
