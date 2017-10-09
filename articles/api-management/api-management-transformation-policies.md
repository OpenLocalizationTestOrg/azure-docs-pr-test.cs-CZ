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
# <a name="api-management-transformation-policies"></a>Transformace zásady služby API Management
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="TransformationPolicies"></a>Zásad transformace  
  
-   [Převést JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON tooXML.  
  
-   [Převod XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML tooJSON.  
  
-   [Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.  
  
-   [Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi hello body tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.  
  
-   [Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny hello back-end službu pro příchozí žádosti.  
  
-   [Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví hello text zprávy pro příchozí a odchozí požadavky.  
  
-   [Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.  
  
-   [Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.  
  
-   [Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby.  
  
-   [Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.  
  
##  <a name="ConvertJSONtoXML"></a>Převést JSON tooXML  
 Hello `json-to-xml` zásad převede text požadavku nebo odpovědi z JSON tooXML.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Příklad  
  
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
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|JSON do xml|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Použít|Hello musí být nastaven tooone hello následující hodnoty.<br /><br /> -vždy - vždy použít převod.<br />obsah json typ – převod pouze v případě, že přítomnost JSON určuje hlavičku odpovědi Content-Type.|Ano|Není k dispozici|  
|Vezměte v úvahu přijmout – hlavička|Hello musí být nastaven tooone hello následující hodnoty.<br /><br /> Pokud se vyžaduje JSON v požadavku hlavička Accept - true – použít převod.<br />-false – vždy použít převod.|Ne|Hodnota TRUE|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozích, odchozích, při chybě  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="ConvertXMLtoJSON"></a>Převést XML tooJSON  
 Hello `xml-to-json` zásad převede text požadavku nebo odpovědi z XML tooJSON. Tuto zásadu lze použít toomodernize rozhraní API založené na webových službách jen XML back-end.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Příklad  
  
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
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|XML – json|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Typ|Hello musí být nastaven tooone hello následující hodnoty.<br /><br /> javascript – procházející - hello převést JSON má popisný tooJavaScript vývojáři formuláře.<br />-direct - hello převedený JSON odráží hello původní XML struktury dokumentu.|Ano|Není k dispozici|  
|Použít|Hello musí být nastaven tooone hello následující hodnoty.<br /><br /> Převeďte – vždy - vždy.<br />obsah typu xml - převést pouze v případě, že hlavičku odpovědi Content-Type označuje přítomnost XML.|Ano|Není k dispozici|  
|Vezměte v úvahu přijmout – hlavička|Hello musí být nastaven tooone hello následující hodnoty.<br /><br /> Pokud se vyžaduje XML v žádosti o hlavička Accept - true – použít převod.<br />-false – vždy použít převod.|Ne|Hodnota TRUE|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozích, odchozích, při chybě  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="Findandreplacestringinbody"></a>Najít a nahradit řetězec v textu  
 Hello `find-and-replace` zásady najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|hledání a nahrazování|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Z|Hello toosearch řetězec pro.|Ano|Není k dispozici|  
|na|Hello náhradní řetězec. Zadejte řetězec nulové délky náhradní řetězec tooremove hello vyhledávání.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="MaskURLSContent"></a>Maska adresy URL v obsahu  
 Hello `redirect-content-urls` zásad znovu zapíše odkazy (masky) v odpovědi hello tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello. V hello odchozí části toore zápisu odpovědi textu odkazy toomake je použijte bod toohello brány. Použití v hello příchozí části opačný efekt.  
  
> [!NOTE]
>  Tato zásada nezmění žádné hodnoty hlavičky, jako `Location` hlavičky. hodnoty hlavičky toochange, použijte hello [sadu hlaviček](api-management-transformation-policies.md#SetHTTPheader) zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|adresy URL přesměrování obsahu|Kořenový element.|Ano|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="SetBackendService"></a>Nastavení back-end služby  
 Použití hello `set-backend-service` zásad tooredirect příchozí žádosti o různých back-end tooa než hello je zadáno v nastavení hello rozhraní API pro tuto operaci. Tato zásada se mění hello back-end služby základní adresu URL hello příchozí požadavek toohello uvedené v zásadách hello.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a>Příklad  
  
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
V tento příklad hello nastavit back-end službu zásady směrování požadavků na základě hodnoty verze hello předaná hello dotazu řetězec tooa různých back-end službu než jeden zadaný v hello hello rozhraní API.
  
Původně hello základní adresu URL back-end služby je odvozená od nastavení hello rozhraní API. Proto hello adrese URL žádosti `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` stane `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` kde `http://contoso.com/api/10.4/` je back-end adresa URL služby hello uvedenou v nastavení hello rozhraní API.  
  
Když hello [< zvolte\> ](api-management-advanced-policies.md#choose) platí prohlášení o zásadách základní adresu URL aplikace hello back-end služby může změnit znovu buď příliš`http://contoso.com/api/8.2` nebo `http://contoso.com/api/9.1`, v závislosti na hello hodnota parametru dotazu požadavku verze hello. Například pokud hello hodnota je `"2013-15"` hello poslední žádosti se změní na adresu URL `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
Pokud transformace hello požadavku je další požadované, ostatní [zásad transformace](api-management-transformation-policies.md#TransformationPolicies) lze použít. Například tooremove hello verze dotazu parametr teď, když hello požadavku se směrují tooa verze konkrétní back-end, hello [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásada se dá použít tooremove hello teď redundantní verze atribut.  
  
### <a name="example"></a>Příklad  
  
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
V tento příklad hello zásad trasy hello požadavek tooa služby fabric back-end používání řetězec dotazu userId hello jako klíč oddílu hello a hello primární repliku hello oddílu.  

### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|set-back-end service|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Základní adresa url|Nový back-end základní adresa URL služby.|Ne|Není k dispozici|  
|id back-end|Identifikátor tooroute back-end hello k.|Ne|Není k dispozici|  
|klíč oddílu SF|Platí jenom při hello back-end je služba Service Fabric a je určen pomocí back-end id. Použít tooresolve na konkrétní oddíl z překládání adres hello.|Ne|Není k dispozici|  
|Typ SF repliky|Platí jenom při hello back-end je služba Service Fabric a je určen pomocí back-end id. Ovládací prvky, pokud požadavek hello by měli přejít toohello primární nebo sekundární replice oddílu. |Ne|Není k dispozici|    
|SF. vyřešte podmínek|Platí jenom při hello back-end je služba Service Fabric. Podmínka identifikace, pokud hello volat back-end Fabric tooService má toobe opakuje s nové řešení.|Ne|Není k dispozici|    
|SF-service-instance-name|Platí jenom při hello back-end je služba Service Fabric. Umožňuje toochange instance služby za běhu. |Ne|Není k dispozici|    

### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí back-end  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="SetBody"></a>Sada textu  
 Použití hello `set-body` zásad tooset hello text zprávy pro příchozí a odchozí požadavky. tělo zprávy, které můžete použít hello hello tooaccess `context.Request.Body` vlastnost nebo hello `context.Response.Body`, v závislosti na tom, zda text hello zásad v hello příchozí nebo odchozí části.  
  
> [!IMPORTANT]
>  Všimněte si, že ve výchozím nastavení při přístupu k hello pomocí tělo zprávy `context.Request.Body` nebo `context.Response.Body`, původní zprávy hello textu dojde ke ztrátě a musí být nastaven vrácením textu hello zpět ve výrazu hello. textu hello toopreserve obsahu, nastavte hello `preserveContent` parametr příliš`true` při přístupu k uvítací zprávu. Pokud `preserveContent` je nastaven příliš`true` a jiný text se vrátí hello výrazem hello vrátil textu se používá.  
>   
>  Upozorňujeme hello následující aspekty při používání hello `set-body` zásad.  
>   
>  -   Pokud používáte hello `set-body` zásad tooreturn nové nebo aktualizované text nepotřebujete tooset `preserveContent` příliš`true` vzhledem k tomu, že jsou explicitně poskytuje nový obsah textu hello.  
> -   Zachování obsahu hello odpovědi v kanálu příchozí hello nebude mít smysl, protože nepřijde žádná odpověď ještě.  
> -   Zachování hello obsahu žádosti v kanálu odchozí hello nebude mít smysl, protože hello žádost již byla odeslána toohello back-end v tomto okamžiku.  
> -   Pokud tato zásada je použita, pokud neexistuje žádný text zprávy, například v příchozí GET, je vyvolána výjimka.  
  
 Další informace najdete v tématu hello `context.Request.Body`, `context.Response.Body`a hello `IMessage` části hello [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) tabulky.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="literal-text-example"></a>Příklad textového literálu  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a>Příklad přístup k textu hello jako řetězec. Všimněte si, původní text žádosti hello jsme se zachováním, tak, aby jsme k němu přístup později v kanálu hello.
  
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
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a>Příklad přístup k textu hello jako JObject. Pamatujte, že vzhledem k tomu, že jsme nejsou rezervování hello původní text žádosti, accesing ho později v kanálu hello způsobí výjimku.  
  
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
  
#### <a name="filter-response-based-on-product"></a>Filtr reakci na produktu  
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

### <a name="using-liquid-templates-with-set-body"></a>Pomocí sady textu kapaliny šablony 
Hello `set-body` zásad může být nakonfigurované toouse hello [kapaliny](https://shopify.github.io/liquid/basics/introduction/) ukázka jazyk tootransfom hello textu požadavku nebo odpovědi. To může být velmi efektivní, pokud potřebujete toocompletely změna tvaru hello formát zprávy.

> [!IMPORTANT]
> Hello implementace kapaliny použít v hello `set-body` v "režimu C#, jsou nakonfigurované zásady. To je zvlášť důležité při provádění akcí, například filtrování. Jako příklad použití Filtr kalendářních dat vyžaduje použití hello Pascal velká a malá písmena a C# datum formátování např:
>
> {{body.foo.startDateTime| Datum: "yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> V pořadí toocorrectly vazby tooan textu XML pomocí hello kapaliny šablony, používat `set-header` zásad tooset Content-Type tooeither application/xml, text/xml (nebo libovolný typ konče + xml); pro text JSON, musí být application/json, text/json (nebo libovolný typ ukončování s + json).

#### <a name="convert-json-toosoap-using-a-liquid-template"></a>Převést JSON tooSOAP pomocí kapaliny šablony
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

#### <a name="tranform-json-using-a-liquid-template"></a>JSON Tranform pomocí kapaliny šablony
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Sada textu|Kořenový element. Obsahuje základní text hello nebo výrazy, které vrací text.|Ano|  

### <a name="properties"></a>Vlastnosti  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|šablona|Použít toochange hello ukázka režim, který hello nastavit tělo zásady se spustí v. Aktuálně hello podporována pouze hodnota je:<br /><br />-kapaliny - hello nastavit tělo zásady bude používat modul kapaliny ukázka hello |Ne|kapaliny|  

Pro přístup k informacím o hello žádosti a odpovědi, můžete šablonu kapaliny hello vázat objekt kontextu tooa s hello následující vlastnosti: <br />
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



### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="SetHTTPheader"></a>Set – hlavička protokolu HTTP  
 Hello `set-header` zásady přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.  
  
 Seznam hlaviček protokolu HTTP se vloží do zprávy HTTP. Při umístění v příchozí kanálu se tato zásada nastaví hello hlavičky protokolu HTTP pro žádost hello předávány toohello cílovou službu. Při umístění v odchozí kanálu se tato zásada nastaví hello hlavičky protokolu HTTP pro hello odpověď odesílanou toohello brány klienta.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="example"></a>Příklad  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Předat kontext informace toohello back-end službu  
 Tento příklad ukazuje, jak zásady tooapply v hello rozhraní API na úrovni toosupply kontextové informace toohello back-end službu. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too10:30. Na 12:10 je ukázku volání operace v hello portál pro vývojáře, kde se můžete podívat hello zásad v práci.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|set – hlavička|Kořenový element.|Ano|  
|hodnota|Určuje hodnotu hello hello záhlaví toobe sady. Pro více záhlaví s hello přidat další stejný název `value` elementy.|Ano|  
  
### <a name="properties"></a>Vlastnosti  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|existuje akce|Určuje jaké akce tootake při hello záhlaví byl již zadán. Tento atribut musí mít jednu z následujících hodnot hello.<br /><br /> -Přepište - hodnotu hello nahradí existující záhlaví hello.<br />-skip - nenahrazuje hello existující hodnotu hlavičky.<br />-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.<br />-delete - odebere hello hlavičky požadavku hello.<br /><br /> Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.|Ne|přepsání|  
|jméno|Určuje název sady toobe záhlaví hello.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="SetQueryStringParameter"></a>Parametr řetězce dotazu sady  
 Hello `set-query-parameter` přidá zásad, nahradí hodnotu, nebo odstranění požadavku parametr řetězce dotazu. Lze použít toopass očekávanou hello back-end službu parametry dotazu, které jsou volitelné nebo nikdy přítomné v žádosti o hello.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="example"></a>Příklad  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Předat kontext informace toohello back-end službu  
 Tento příklad ukazuje, jak zásady tooapply v hello rozhraní API na úrovni toosupply kontextové informace toohello back-end službu. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too10:30. Na 12:10 je ukázku volání operace v hello portál pro vývojáře, kde se můžete podívat hello zásad v práci.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|parametr dotazu pro sadu|Kořenový element.|Ano|  
|hodnota|Určuje hodnotu hello sady toobe parametr dotazu hello. Pro více parametry dotazu s hello přidat další stejný název `value` elementy.|Ano|  
  
### <a name="properties"></a>Vlastnosti  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|existuje akce|Jaké akce tootake Určuje, pokud již zadán parametr dotazu hello. Tento atribut musí mít jednu z následujících hodnot hello.<br /><br /> -Přepište - hodnota hello nahradí existující parametru hello.<br />-skip - nenahrazuje hello existující hodnota parametru dotazu.<br />-připojit - připojí hello hodnota toohello existující hodnota parametru dotazu.<br />-delete - odebere hello parametr dotazu z požadavku hello.<br /><br /> Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v parametru dotazu hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.|Ne|přepsání|  
|jméno|Určuje název sady toobe parametr dotazu hello.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí back-end  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="RewriteURL"></a>Přepsání adresy URL  
 Hello `rewrite-uri` zásad převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby, jak ukazuje následující příklad hello.  
  
-   Veřejnou adresu URL-`http://api.example.com/storenumber/ordernumber`  
  
-   Adresa URL požadavku –`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Tuto zásadu lze použít, když lidského nebo prohlížeče friendly adresa URL by měla transformuje se na formát adresy URL hello očekávanou hello webové služby. Tato zásada, stačí toobe použít při vystavení alternativní formátu adresy URL, například vyčištění adresy URL, RESTful adresy URL, uživatelsky přívětivý adresy URL SEO přátelské adresy URL, které jsou čistě strukturální adresy URL, které nebude obsahovat řetězec dotazu a místo toho obsahují pouze hello cestu hello prostředek (po hello schématu a autoritě hello). To se často provádí estetické, použitelnost nebo vyhledávací web pro účely optimalizace (vyhledávací weby SEO).  
  
> [!NOTE]
>  Pouze můžete přidat pomocí zásad hello parametrů řetězce dotazu. Nelze přidat další šablony cestou parametry v hello přepisu adresy URL.  

### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a>Příklad  
  
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

### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Přepište uri|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Atribut|Popis|Požaduje se|Výchozí|  
|---------------|-----------------|--------------|-------------|  
|šablona|Hello adresa URL skutečné webové služby s všech parametrů řetězce dotazu. Pokud používáte výrazy, hello celou hodnota musí být výraz.|Ano|Není k dispozici|  
|kopírování neodpovídající parametry|Určuje, zda jsou parametry dotazu v hello příchozí žádosti nejsou k dispozici v původní šabloně URL hello přidány toohello URL definované hello znovu zapsat šablony|Ne|Hodnota TRUE|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** produktu, rozhraní API, operace  
  
##  <a name="XSLTransform"></a>Transformace XML pomocí transformaci XSLT  
 Hello `Transform XML using an XSLT` zásada se vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
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
  
### <a name="example"></a>Příklad  
  
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
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|transformace XSL|Kořenový element.|Ano|  
|Parametr|Použít toodefine proměnné používané v hello transformace|Ne|  
|: stylesheet|Kořenový element šablony stylů. Všechny elementy a atributy definované v rámci postupujte podle hello standard [specifikace XSLT](http://www.w3.org/TR/xslt)|Ano|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  
