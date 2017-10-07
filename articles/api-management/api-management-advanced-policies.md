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
# <a name="api-management-advanced-policies"></a>Pokročilé zásady API Management
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AdvancedPolicies"></a>Rozšířené zásady  
  
-   [Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad na základě výsledků hello hello vyhodnocení logická hodnota [výrazy](api-management-policy-expressions.md).  
  
-   [Předat dál žádost](#ForwardRequest) -předává hello požadavek toohello back-end službu.

-   [Omezit souběžnosti](#LimitConcurrency) -brání uzavřena zásady z provádění více než hello určitý počet požadavků současně.
  
-   [Protokolu tooEvent rozbočovače](#log-to-eventhub) -odešle zprávy v hello zadaný formát tooan centra událostí definované entity protokolovacího nástroje. 

-   [Model odpovědi](#mock-response) -přerušení způsobených kanálu provádění a vrátí odpověď mocked přímo toohello volajícího.
  
-   [Opakujte](#Retry) -provádění opakování hello uzavřené příkazy zásad, pokud a dokud je splněna podmínka hello. Spuštění se bude opakovat v hello zadaným časovým intervalům a až toohello zadaný počet opakování.  
  
-   [Vrátí odpověď](#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí hello zadané odpovědi přímo toohello volajícího. 
  
-   [Odeslání žádosti o jednorázové jednoduché](#SendOneWayRequest) -odešle žádost toohello zadané adresy URL bez čekání na odpověď.  
  
-   [Odeslání požadavku](#SendRequest) -odešle žádost toohello zadaná adresa URL.  

-   [Nastavení proxy serveru HTTP](#SetHttpProxy) -vám umožní tooroute předané požadavky prostřednictvím proxy serveru HTTP.  

-   [Nastaví metodu požadavku](#SetRequestMethod) -vám umožní toochange hello HTTP metoda pro žádost.  
  
-   [Nastavit stavový kód](#SetStatus) -toohello kód stavu hello HTTP změny zadané hodnotě.  
  
-   [Nastavená proměnná](api-management-advanced-policies.md#set-variable) -potrvají hodnotu v pojmenovaná [kontextu](api-management-policy-expressions.md#ContextVariables) proměnná pro pozdější přístup.  

-   [Trasování](#Trace) -přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.  
  
-   [Počkejte](#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady toocomplete než budete pokračovat.  
  
##  <a name="choose"></a>Tok řízení  
 Hello `choose` zásada závorkách zásad příkazy podle hello výsledek vyhodnocení logické výrazy, podobně jako v případě pak jinak tooan nebo přepínač vytvořit v programovacím jazyce.  
  
###  <a name="ChoosePolicyStatement"></a>Prohlášení o zásadách  
  
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
  
 Hello zásad řízení toku musí obsahovat alespoň jeden `<when/>` elementu. Hello `<otherwise/>` prvek je volitelný. Podmínky v `<when/>` elementy jsou vyhodnocovány v pořadí podle jejich vzhled v rámci zásad hello. Příkazy zásad uzavřené v rámci hello nejprve `<when/>` element s podmínku atributu rovná `true` se použijí. Zásady v rámci hello uzavřené `<otherwise/>` elementu, pokud existuje, bude použito v případě všech z hello `<when/>` element podmínku atributy jsou `false`.  
  
### <a name="examples"></a>Příklady  
  
####  <a name="ChooseExample"></a>Příklad  
 Hello následující příklad ukazuje [nastavená proměnná](api-management-advanced-policies.md#set-variable) zásady a dvě zásady řízení toku.  
  
 Hello nastavit proměnnou zásady je v hello příchozí části a vytvoří `isMobile` Boolean [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena tootrue, pokud hello `User-Agent` žádosti záhlaví obsahuje hello text `iPad` nebo `iPhone`.  
  
 zásada toku řízení první Hello je taky v hello příchozí části a podmíněně použije jednu ze dvou [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásady v závislosti na hodnotu hello hello `isMobile` kontextové proměnné.  
  
 Hello druhý řízení toku zásady je v části odchozí hello a podmíněně platí hello [převést XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) zásady při `isMobile` je nastaven příliš`true`.  
  
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
  
#### <a name="example"></a>Příklad  
 Tento příklad ukazuje, jak tooperform filtrování obsahu odebráním datové prvky. z odpovědi hello přijal od služby back-end hello při použití hello `Starter` produktu. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too34:30. Spuštění 31:50 toosee přehled [hello tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|Zvolte|Kořenový element.|Ano|  
|Kdy|Hello toouse podmínku pro hello `if` nebo `ifelse` částí hello `choose` zásad. Pokud hello `choose` zásad obsahuje více `when` oddíly, jsou vyhodnocovány postupně. Jednou hello `condition` systému v případě elementu vyhodnotí příliš`true`, žádné další `when` vyhodnocení podmínek.|Ano|  
|v opačném případě|Obsahuje hello zásad fragment kódu toobe použít, pokud žádná z hello `when` podmínky vyhodnotit příliš`true`.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|  
|---------------|-----------------|--------------|  
|podmínka = "logický výraz &#124; Logická hodnota konstanta"|logický výraz nebo konstantní tooevaluated při hello obsahující Hello `when` zásad vyhodnotí.|Ano|  
  
###  <a name="ChooseUsage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="ForwardRequest"></a>Předat dál požadavku  
 Hello `forward-request` zásada předá hello příchozí požadavek toohello back-end službu určený v požadavku hello [kontextu](api-management-policy-expressions.md#ContextVariables). Hello adresa URL back-end služby je zadaný v hello rozhraní API [nastavení](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) a můžete změnit pomocí hello [nastavení back-end služby](api-management-transformation-policies.md) zásad.  
  
> [!NOTE]
>  Odebrání výsledkem požadavku hello nebudou předávány zásady služby a hello back-end toohello v části odchozí hello zásad se vyhodnocují hned po úspěšném hello hello zásad v hello příchozí části.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="example"></a>Příklad  
 Hello následující zásadou na úrovni rozhraní API předává všechny požadavky toohello back-end službu s časový limit na 60 sekund.  
  
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
  
#### <a name="example"></a>Příklad  
 Tato zásada na úrovni operace používá hello `base` element tooinherit hello back-end zásady z hello nadřazené API úrovni oboru.  
  
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
  
#### <a name="example"></a>Příklad  
 Tato zásada na úrovni operaci explicitně předává všechny požadavky toohello back-end službu s časovým limitem 120 a nedědí hello nadřazené úrovně back-end zásada rozhraní API.  
  
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
  
#### <a name="example"></a>Příklad  
 Tato zásada na úrovni operaci nepředává požadavky toohello back-end službu.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|předání požadavku|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|časový limit = "celé číslo"|Hello interval časového limitu v sekundách, než hello volání toohello back-end službu se nezdaří.|Ne|Bez časového limitu|  
|postupujte podle přesměrování = "true &#124; false"|Určuje, zda jsou přesměrování z back-end službu hello následuje hello brány nebo vrátil toohello volajícího.|Ne|False|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** back-end  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="LimitConcurrency"></a>Limit souběžnosti  
 Hello `limit-concurrency` zásady zabrání závorkách zásady provádění více než hello zadaný počet požadavků, které v daném okamžiku. Při překročení prahové hodnoty hello, přidají nové žádosti o tooa fronty, dokud nedosáhnete hello maximální délka fronty. Po vyčerpání fronty nové požadavky selže okamžitě.
  
###  <a name="LimitConcurrencyStatement"></a>Prohlášení o zásadách  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Příklady  
  
####  <a name="ChooseExample"></a>Příklad  
 Hello následující příklad ukazuje, jak toolimit počet požadavků, které přesměrovávají tooa back-end na základě hodnoty hello kontextové proměnné.
 
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

### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|    
|limit souběžnosti|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|--------------|  
|key|Řetězec. Výraz povoleny. Určuje obor souběžnosti hello. Může být sdílen více zásad.|Ano|Není k dispozici|  
|maximální počet|Celé číslo. Určuje maximální počet požadavků, které jsou povoleny tooenter hello zásad.|Ano|Není k dispozici|  
|timeout|Celé číslo. Výraz povoleny. Určuje hello sekundách žádost čekat tooenter obor než selže s "403 příliš mnoho požadavků"|Ne|Infinity|  
|Maximální délka fronty|Celé číslo. Výraz povoleny. Určuje hello maximální délka fronty. Příchozí požadavky při tooenter tuto zásadu bude ukončena s "403 příliš mnoho požadavků" okamžitě po vyčerpání hello fronty.|Ne|Infinity|  
  
###  <a name="ChooseUsage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  

##  <a name="log-to-eventhub"></a>Protokol tooEvent rozbočovače  
 Hello `log-to-eventhub` zásad odesílá zprávy v hello zadaný formát tooan centra událostí, které jsou definované entity protokolovacího nástroje. Jak již název napovídá, hello zásada se používá pro uložení vybraného požadavku nebo odpovědi kontextové informace pro analýzu online nebo offline.  
  
> [!NOTE]
>  Podrobné informace o konfiguraci centra událostí a protokolování událostí najdete v tématu [jak toolog události API Management s Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Příklad  
 Libovolný řetězec, můžete použít jako toobe hodnota hello přihlášení služby Event Hubs. V tomto příkladu hello datum a čas, název služby pro nasazení, id žádosti, ip adresu a název operace pro všechny příchozí volání jsou centra událostí zaznamenané toohello protokolovacího nástroje zaregistrována hello `contoso-logger` id.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|protokol eventhub|Kořenový element. Hodnota Hello tohoto elementu je centra událostí tooyour toolog řetězec hello.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|  
|---------------|-----------------|--------------|  
|id protokolovacího nástroje|Hello id hello protokolovacího nástroje registrován u služby API Management.|Ano|  
|id oddílu|Určuje index hello hello oddílu, které jsou odesílány zprávy.|Volitelné. Tento atribut nemusí být použit, pokud `partition-key` se používá.|  
|klíč oddílu|Určuje hodnotu hello slouží k přiřazení k oddílu, odesílání zpráv.|Volitelné. Tento atribut nemusí být použit, pokud `partition-id` se používá.|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  

##  <a name="mock-response"></a>Imitované odpovědi  
Hello `mock-response`, jako název hello znamená, že je použit toomock rozhraní API a operace. Se zruší provádění běžný kanál a vrátí volající toohello mocked odpovědi. Hello zásad se vždy pokusí tooreturn odpovědí nejvyšší přesnosti. Dává přednost obsahu příklady odpovědi, vždy, když je k dispozici. Generuje ukázka odpovědí z schémata, když jsou k dispozici schémata a příklady nejsou. Pokud ani příklady nebo schémata jsou nalezena, vrátí se odpovědi s žádný obsah.
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Příklady  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|model odpovědi|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|--------------|  
|Stavový kód|Určuje stavový kód odpovědi a je použité tooselect odpovídající příklad nebo schéma.|Ne|200|  
|Typ obsahu|Určuje `Content-Type` hodnota hlavičky odpovědi a odpovídající příklad použité tooselect nebo schéma.|Ne|Žádný|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozích, odchozích, při chybě  
  
-   **Zásady obory:** všechny obory

##  <a name="Retry"></a>Opakování  
 Hello `retry` zásady provede jeho podřízených zásady jednou a pak opakuje jejich spuštění, dokud hello opakování `condition` stane `false` nebo opakujte `count` je vyčerpání.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
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
  
### <a name="example"></a>Příklad  
 V hello se pokus o následující příklad požadavek forewarding až tooten časy pomocí algoritmu exponenciální opakování. Vzhledem k tomu `first-fast-retry` nastavena toofalse, všechny pokusy jsou subjektu toohello exponsntial opakování algoritmus.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|retry|Kořenový element. Může obsahovat další zásady jako jeho podřízených elementů.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|Podmínka|Logická hodnota literál nebo [výraz](api-management-policy-expressions.md) zadání, pokud by se měla zastavit opakovaných pokusů (`false`) nebo pokračování (`true`).|Ano|Není k dispozici|  
|Počet|Kladné číslo určující maximální počet opakování tooattempt hello.|Ano|Není k dispozici|  
|interval|Kladné číslo v sekundách zadání intervalu čekání hello mezi pokusy o opakování hello.|Ano|Není k dispozici|  
|maximální interval|Kladné číslo v sekundách zadání hello maximální čekání interval mezi pokusy o opakování hello. Je použité tooimplement algoritmu exponenciální opakování.|Ne|Není k dispozici|  
|rozdílů|Kladné číslo v sekundách zadání přírůstek intervalu čekání hello. Je použité tooimplement hello lineární a exponenciální opakování algoritmy.|Ne|Není k dispozici|  
|Zkuste zopakovat první fast|Pokud nastavení příliš `true` , hello první pokus o opakování provádí okamžitě.|Ne|`false`|  
  
> [!NOTE]
>  Když pouze hello `interval` není zadaný, **pevné** jsou prováděny interval opakování.  
>  Když pouze hello `interval` a `delta` nejsou zadány, **lineární** se použije algoritmus interval opakování, kde je doba čekání mezi opakovanými pokusy počítané podle hello následující vzorec - `interval + (count - 1)*delta`.  
>  Když hello `interval`, `max-interval` a `delta` jsou nastaveny, **exponenciální** interval opakování algoritmus se použije, kde je doba čekání hello mezi opakovanými pokusy hello zvětšování exponenciálně od hodnoty hello `interval`toohello hodnotu `max-interval` podle následující toohello forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Všimněte si, že podřízená omezení použití zásad zdědí tuto zásadu.  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="ReturnResponse"></a>Vrátí odpověď  
 Hello `return-response` zásady zruší provádění zřetězeného příkazu a vrací výchozí nebo vlastní odpovědi toohello volajícího. Výchozí odpověď je `200 OK` se žádné textem. Vlastní odpovědi lze zadat pomocí kontextu proměnné nebo zásady příkazy. Pokud obě jsou k dispozici, hello odpovědi obsažené v hello kontextové proměnné je upravit příkazy zásad hello před vrácením toohello volajícího.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|vrátí odpověď|Kořenový element.|Ano|  
|set – hlavička|A [sadu hlaviček](api-management-transformation-policies.md#SetHTTPheader) prohlášení o zásadách.|Ne|  
|Sada textu|A [set textu](api-management-transformation-policies.md#SetBody) prohlášení o zásadách.|Ne|  
|Nastavit stav|A [nastavit stav](api-management-advanced-policies.md#SetStatus) prohlášení o zásadách.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|  
|---------------|-----------------|--------------|  
|Název proměnné odpovědi|Hello název kontextové proměnné hello na něj odkazovat z, například předcházejícího [odeslán požadavek](api-management-advanced-policies.md#SendRequest) zásady a obsahující `Response` objektu|Volitelné.|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="SendOneWayRequest"></a>Odeslání žádosti o jednorázové jednoduché  
 Hello `send-one-way-request` zásad odešle požadavek hello poskytuje toohello zadané adresy URL bez čekání na odpověď.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Příklad  
 Tato ukázková zásada ukazuje příklad použití hello `send-one-way-request` toosend zásad v zpráva tooa Slack chatovací místnosti Pokud hello kódu odpovědi HTTP je větší než nebo rovna too500. Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|odeslání jeden způsob požadavků|Kořenový element.|Ano|  
|Adresa URL|Adresa URL Hello hello požadavku.|Pokud žádné režimu = kopie; v opačném případě Ano.|  
|– Metoda|Metoda HTTP pro žádost o hello Hello.|Pokud žádné režimu = kopie; v opačném případě Ano.|  
|záhlaví|Hlavička požadavku. Použijte více prvky záhlaví pro více hlavičky žádosti.|Ne|  
|Text|datová část požadavku Hello.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|režim = "řetězec"|Určuje, jestli je nový požadavek nebo kopii hello aktuální požadavek. V odchozí režim režim = kopie neinicializuje hello textu požadavku.|Ne|Nový|  
|jméno|Určuje název hello hello záhlaví toobe sady.|Ano|Není k dispozici|  
|existuje akce|Určuje jaké akce tootake při hello záhlaví byl již zadán. Tento atribut musí mít jednu z následujících hodnot hello.<br /><br /> -Přepište - hodnotu hello nahradí existující záhlaví hello.<br />-skip - nenahrazuje hello existující hodnotu hlavičky.<br />-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.<br />-delete - odebere hello hlavičky požadavku hello.<br /><br /> Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.|Ne|přepsání|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="SendRequest"></a>Odeslání požadavku  
 Hello `send-request` zásad odešle požadavek hello poskytuje toohello zadat adresu URL, už čeká, než hello nastavit hodnotu časového limitu.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Příklad  
 Tento příklad ukazuje jeden ze způsobů tooverify token odkazu se serverem ověřování. Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|odeslání žádosti|Kořenový element.|Ano|  
|Adresa URL|Adresa URL Hello hello požadavku.|Pokud žádné režimu = kopie; v opačném případě Ano.|  
|– Metoda|Metoda HTTP pro žádost o hello Hello.|Pokud žádné režimu = kopie; v opačném případě Ano.|  
|záhlaví|Hlavička požadavku. Použijte více prvky záhlaví pro více hlavičky žádosti.|Ne|  
|Text|datová část požadavku Hello.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|režim = "řetězec"|Určuje, jestli je nový požadavek nebo kopii hello aktuální požadavek. V odchozí režim režim = kopie neinicializuje hello textu požadavku.|Ne|Nový|  
|Název proměnné odpovědi = "řetězec"|Pokud není přítomný, `context.Response` se používá.|Ne|Není k dispozici|  
|časový limit = "celé číslo"|Hello interval časového limitu v sekundách, než hello volání toohello adresa URL se nezdaří.|Ne|60|  
|Ignorovat chybu|Pokud hodnotu true a hello požadavku dojde k chybě:<br /><br /> – Pokud je název proměnné odpověď byla zadána, bude obsahovat hodnotu null.<br />– Pokud nebyl zadán název proměnné odpovědi, kontextu. Požadavek nebude aktualizován.|Ne|False|  
|jméno|Určuje název hello hello záhlaví toobe sady.|Ano|Není k dispozici|  
|existuje akce|Určuje jaké akce tootake při hello záhlaví byl již zadán. Tento atribut musí mít jednu z následujících hodnot hello.<br /><br /> -Přepište - hodnotu hello nahradí existující záhlaví hello.<br />-skip - nenahrazuje hello existující hodnotu hlavičky.<br />-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.<br />-delete - odebere hello hlavičky požadavku hello.<br /><br /> Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.|Ne|přepsání|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="SetHttpProxy"></a>Server proxy protokolu HTTP sady  
 Hello `proxy` zásad vám umožní tooroute požadavky předané toobackends prostřednictvím proxy serveru HTTP. Mezi hello brány a hello proxy je podporována jen protokol HTTP (nikoli HTTPS). Pouze ověřování NTLM a Basic.
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Příklad  
Všimněte si použití hello [vlastnosti](api-management-howto-properties.md) jako hodnoty hello uživatelské jméno a heslo tooavoid ukládání citlivých informací v dokumentu zásad hello.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|Proxy server|Kořenový element|Ano|  

### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|Adresa URL = "řetězec"|Adresa URL proxy serveru v hello formu http://host:port.|Ano|Není k dispozici|  
|uživatelské jméno = "řetězec"|Uživatelské jméno toobe používá k ověřování připojení k proxy serveru hello.|Ne|Není k dispozici|  
|heslo = "řetězec"|Toobe heslo použít k ověřování připojení k proxy serveru hello.|Ne|Není k dispozici|  

### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** všechny obory  

##  <a name="SetRequestMethod"></a>Žádost o set – metoda  
 Hello `set-method` zásad vám umožní metoda žádosti toochange hello HTTP pro žádost.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Příklad  
 Tato ukázka zásadu, která používá hello `set-method` zásad ukazuje příklad odesílání Slack chatovací místnosti tooa zprávu, pokud hello kódu odpovědi HTTP je větší než nebo rovna too500. Další informace o této ukázky najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|set – metoda|Kořenový element. Hodnota Hello elementu hello určuje metoda HTTP hello.|Ano|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="SetStatus"></a>Sada stavový kód  
 Hello `set-status` zásad sady hello HTTP stavový kód toohello zadané hodnotě.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Příklad  
 Tento příklad ukazuje, jak tooreturn odpovědi 401, pokud hello autorizační token je neplatný. Další informace najdete v tématu [pomocí externích služeb z hello služby Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|Nastavit stav|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|kód = "celé číslo"|tooreturn kód stavu Hello HTTP.|Ano|Není k dispozici|  
|důvod = "řetězec"|Popis hello důvod pro vrácení hello stavový kód.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** odchozí, back-end, při chybě  
  
-   **Zásady obory:** všechny obory  

##  <a name="set-variable"></a>Nastavená proměnná  
 Hello `set-variable` deklaruje zásad [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné a přiřadí ji má hodnotu prostřednictvím [výraz](api-management-policy-expressions.md) nebo řetězcový literál. Pokud hello výraz obsahuje literál se převede tooa řetězec a hello typ hodnoty hello bude `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a>Prohlášení o zásadách  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a>Příklad  
 Hello následující příklad ukazuje nastavení proměnné zásad v hello příchozí části. Toto nastavení zásad proměnné vytvoří `isMobile` Boolean [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné, která je nastavena tootrue, pokud hello `User-Agent` žádosti záhlaví obsahuje hello text `iPad` nebo `iPhone`.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|nastavená proměnná|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|  
|---------------|-----------------|--------------|  
|jméno|Hello název proměnné hello.|Ano|  
|hodnota|Hodnota Hello hello proměnné. To může být výraz nebo hodnota literálu.|Ano|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
###  <a name="set-variableAllowedTypes"></a>Povolené typy  
 Výrazy použité v hello `set-variable` zásady musí vrátit jednu z následujících základních typů hello.  
  
-   System.Boolean  
  
-   System.SByte  
  
-   System.Byte  
  
-   System.UInt16  
  
-   System.UInt32  
  
-   System.UInt64  
  
-   System.Int16  
  
-   System.Int32  
  
-   System.Int64  
  
-   System.Decimal  
  
-   System.Single  
  
-   System.Double  
  
-   System.Guid  
  
-   System.String  
  
-   System.Char  
  
-   System.DateTime  
  
-   System.TimeSpan  
  
-   System.Byte?  
  
-   System.UInt16?  
  
-   System.UInt32?  
  
-   System.UInt64?  
  
-   System.Int16?  
  
-   System.Int32?  
  
-   System.Int64?  
  
-   System.Decimal?  
  
-   System.Single?  
  
-   System.Double?  
  
-   System.Guid?  
  
-   System.String?  
  
-   System.Char?  
  
-   System.DateTime?  

##  <a name="Trace"></a>Trasování  
 Hello `trace` zásad přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup. Hello zásady budou spuštěny pouze při trasování je aktivaci, tj. `Ocp-Apim-Trace` hlavička požadavku je přítomen a nastavte příliš`true` a `Ocp-Apim-Subscription-Key` hlavička požadavku je k dispozici a obsahuje platný klíč přidružený k účtu správce hello.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|trasování|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|Zdroj|Řetězcový literál smysluplný toohello trasování prohlížeč a zadání hello zdroj zprávy hello.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** všechny obory  
  
##  <a name="Wait"></a>Počkej  
 Hello `wait` zásady spustí své zásady bezprostředně podřízené paralelně a čeká všechny nebo jednoho z jeho toocomplete zásady bezprostředně podřízené před dokončením. Hello čekání zásad může mít jako jeho bezprostředně podřízené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), a [řízení toku](api-management-advanced-policies.md#choose) zásady.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Příklad  
 V následující ukázka hello jsou uvedeny dvě `choose` zásady jako bezprostředně podřízené zásady hello `wait` zásad. Každá z těchto `choose` zásady spustí paralelně. Každý `choose` zásad pokusí tooretrieve hodnotu uloženou v mezipaměti. Pokud dojde k neúspěšnému přístupu do mezipaměti, se nazývá back-end službu tooprovide hello hodnotu. V tento příklad hello `wait` zásad dokončena, dokud se všechny jeho podřízené okamžitou zásady dokončit, protože hello `for` atribut je nastaven příliš`all`.   V této proměnné kontextu hello příklad (`execute-branch-one`, `value-one`, `execute-branch-two`, a `value-two`) jsou mimo rozsah hello tento příklad zásady deklarován.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|Počkej|Kořenový element. Může obsahovat pouze podřízené elementy `send-request`, `cache-lookup-value`, a `choose` zásady.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|Pro|Určuje, zda hello `wait` zásad čeká na všechny zásady toobe bezprostředně podřízené dokončit nebo jenom jeden. Povolené hodnoty jsou:<br /><br /> -   `all`-čekat na všechny zásady toocomplete bezprostředně podřízené<br />-všechny - počkejte žádné zásady toocomplete bezprostředně podřízené. Po dokončení hello první bezprostředně podřízené zásad hello `wait` zásad dokončí a provádění dalších zásad bezprostředně podřízené je ukončen.|Ne|Všechny|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end  
  
-   **Zásady obory:**všechny obory  
  
## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu:
-   [Zásady ve službě API Management](api-management-howto-policies.md) 
-   [Výrazy zásad](api-management-policy-expressions.md)
