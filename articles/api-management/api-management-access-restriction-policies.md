---
title: "zásady omezení přístupu aaaAzure API Management | Microsoft Docs"
description: "Další informace o zásadách omezení přístupu hello k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 0ef368c2781d9a5cf9eaaa41a47489c904ed3198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-access-restriction-policies"></a>Zásady omezení přístupu služby API Management
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a>Zásady omezení přístupu  
  
-   [Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.  
  
-   [Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.  
  
-   [Omezení četnosti volání podle klíče](#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.  
  
-   [Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.  
  
-   [Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.  
  
-   [Nastavení kvóty využití podle klíče](#SetUsageQuotaByKey) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.  
  
-   [Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.  
  
##  <a name="CheckHTTPHeader"></a>Zkontrolujte hlavičky protokolu HTTP  
 Použití hello `check-header` tooenforce zásady, že požadavek je zadaná hlavička protokolu HTTP. Volitelně můžete zkontrolovat toosee Pokud hlavička hello obsahuje konkrétní hodnotu nebo zkontrolujte rozsah povolených hodnot. Pokud kontrola hello selže, zásady hello ukončí zpracování žádosti a vrátí hello HTTP stavový kód a chybové zprávy, určeného hello zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Kontrola – hlavička|Kořenový element.|Ano|  
|hodnota|Povolená hodnota hlavičky protokolu HTTP. Když je zadaná víc elementů hodnotu, zkontrolujte hello je považován za úspěšné, pokud kterékoli z hodnoty hello je nalezena shoda.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|se nezdařila zkontrolujte chybových zpráv|Chybová zpráva tooreturn v textu odpovědi hello HTTP, pokud hlavička hello neexistuje nebo má neplatnou hodnotu. Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.|Ano|Není k dispozici|  
|se nezdařilo httpcode kontrola|Stav protokolu HTTP kód tooreturn Pokud hello záhlaví neexistuje nebo má neplatnou hodnotu.|Ano|Není k dispozici|  
|název hlavičky|Název Hello hello toocheck hlavičky protokolu HTTP.|Ano|Není k dispozici|  
|Ignorovat případu|Můžete nastavit, tooTrue, nebo hodnotu NEPRAVDA. Pokud se ignoruje velikost písmen tooTrue sady při hodnota hlavičky hello se porovná s hello sadu přijatelných hodnot.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="LimitCallRate"></a>Omezení četnosti volání podle předplatného  
 Hello `rate-limit` zásada brání použití rozhraní API špičky na základě předplatného za omezením hello volání míra tooa zadané číslo za zadané časové období. Když se aktivuje tuto zásadu obdrží hello volajícího `429 Too Many Requests` stavový kód odpovědi.  
  
> [!IMPORTANT]
>  Tuto zásadu lze použít jenom jednou za zásady dokumentu.  
>   
>  [Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Nastavte limit|Kořenový element.|Ano|  
|rozhraní api|Přidáte jeden nebo více z těchto elementů tooimpose omezení četnosti volání na rozhraní API v rámci produktu hello. Produkt a rozhraní API volat rychlost, kterou omezení platí nezávisle.|Ne|  
|operace|Přidáte jeden nebo více z těchto elementů tooimpose omezení četnosti volání na operace v rámci rozhraní API. Produktu, rozhraní API a operace četnosti, kterou omezení platí nezávisle.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|jméno|Hello název hello rozhraní API pro omezit rychlost tooapply hello.|Ano|Není k dispozici|  
|volání|maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.|Ano|Není k dispozici|  
|období obnovení|Hello doba v sekundách, po které hello kvóta resetuje.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** produktu  
  
##  <a name="LimitCallRateByKey"></a>Omezení četnosti volání podle klíče  
 Hello `rate-limit-by-key` zásada brání použití rozhraní API špičky na základě za klíč omezením hello volání míra tooa zadané číslo za zadané časové období. klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad. Volitelné Přírůstek podmínku můžete přidat toospecify, které požadavky mělo být započítáno směrem hello limit. Když se aktivuje tuto zásadu obdrží hello volajícího `429 Too Many Requests` stavový kód odpovědi.  
  
 Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Tuto zásadu lze použít jenom jednou za zásady dokumentu.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Příklad  
 V následujícím příkladu hello omezení četnosti hello se ukládá do klíčů pomocí hello volající IP adresy.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Nastavte limit|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|volání|maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.|Ano|Není k dispozici|  
|kontrolní klíč|Hello klíče toouse pro zásady omezení četnosti hello.|Ano|Není k dispozici|  
|Increment – podmínky|logický výraz Hello zadání, pokud požadavek hello mělo být započítáno směrem hello kvóty (`true`).|Ne|Není k dispozici|  
|období obnovení|Hello doba v sekundách, po které hello kvóta resetuje.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="RestrictCallerIPs"></a>Omezit volající IP adresy  
 Hello `ip-filter` zásad filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Filtr IP|Kořenový element.|Ano|  
|Adresa|Určuje jednu IP adresu, na které toofilter.|Alespoň jeden `address` nebo `address-range` prvek je nutný.|  
|rozsah adres z = "address" k = "address"|Určuje rozsah IP adres, na které toofilter.|Alespoň jeden `address` nebo `address-range` prvek je nutný.|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|rozsah adres z = "address" k = "address"|Rozsah IP adres tooallow nebo odepřít přístup.|Požadováno, když hello `address-range` element se používá.|Není k dispozici|  
|Filtr IP akce = "Povolit &#124; nezakazuje"|Určuje, zda se má povolit volání, nebo není pro hello zadané IP adresy a rozsahy.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="SetUsageQuota"></a>Nastavení kvóty využití podle předplatného  
 Hello `quota` zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.  
  
> [!IMPORTANT]
>  Tuto zásadu lze použít jenom jednou za zásady dokumentu.  
>   
>  [Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|kvóta|Kořenový element.|Ano|  
|rozhraní api|Přidáte jeden nebo více z těchto elementů tooimpose kvótu na rozhraní API v rámci produktu hello. Produkt a rozhraní API kvóty platí nezávisle.|Ne|  
|operace|Přidáte jeden nebo více z těchto elementů tooimpose kvótu na operace v rámci rozhraní API. Kvóty produktu, rozhraní API a operace platí nezávisle.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|jméno|Hello název rozhraní API hello nebo operaci, pro které hello kvóty platí.|Ano|Není k dispozici|  
|Šířka pásma|Hello maximální celkový počet kilobajtů povolené během časového intervalu hello zadaný v hello `renewal-period`.|Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.|Není k dispozici|  
|volání|maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.|Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.|Není k dispozici|  
|období obnovení|Hello doba v sekundách, po které hello kvóta resetuje.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** produktu  
  
##  <a name="SetUsageQuotaByKey"></a>Nastavení kvóty využití podle klíče  
 Hello `quota-by-key` zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč. klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad. Volitelné Přírůstek podmínku můžete přidat toospecify, které požadavky mělo být započítáno směrem hello kvóty.  
  
 Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Tuto zásadu lze použít jenom jednou za zásady dokumentu.  
>   
>  [Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Příklad  
 V následujícím příkladu hello kvóty hello se ukládá do klíčů pomocí hello volající IP adresy.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|kvóta|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Šířka pásma|Hello maximální celkový počet kilobajtů povolené během časového intervalu hello zadaný v hello `renewal-period`.|Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.|Není k dispozici|  
|volání|maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.|Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.|Není k dispozici|  
|kontrolní klíč|Hello klíče toouse hello zásady kvót.|Ano|Není k dispozici|  
|Increment – podmínky|logický výraz Hello zadání, pokud požadavek hello mělo být započítáno směrem hello kvóty (`true`)|Ne|Není k dispozici|  
|období obnovení|Hello doba v sekundách, po které hello kvóta resetuje.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
##  <a name="ValidateJWT"></a>Ověřit token JWT  
 Hello `validate-jwt` zásady vynucují existence a platnosti token JWT extrahovat z buď zadané hlavičky protokolu HTTP nebo parametr zadaný dotaz.  
  
> [!IMPORTANT]
>  Hello `validate-jwt` zásad vyžaduje tento hello `exp` není registrovaný deklarace inlcuded v tokenu JWT hello, pokud `require-expiration-time` atribut je zadán a nastavit příliš`false`.  
> Hello `validate-jwt` zásad podporuje HS256 a RS256 podpisový algoritmy. Pro HS256 hello klíč je třeba zadat ve vloženém hello zásady v podobě hello kódováním base64. Pro RS256 hello klíč má toobe zadejte přes koncový bod Open ID konfigurace.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<validate-jwt   
    header-name="name of http header containing hello token (use query-parameter-name attribute if hello token is passed in hello URL)"   
    failed-validation-httpcode="http status code tooreturn on failure"   
    failed-validation-error-message="error message tooreturn on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of hello claim as it appears in hello token" match="all|any">  
      <value>claim value as it is expected tooappear in hello token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of hello configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="azure-mobile-services-token-validation"></a>Ověření tokenu Azure Mobile Services  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Ověření tokenu Azure Active Directory  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Ověření tokenu Azure Active Directory B2C  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a>Autorizovat toooperations přístupu na základě tokenu deklarací  
 Tento příklad ukazuje, jak toouse hello [ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) zásad toopre-toooperations přístupu na základě tokenu deklarací autorizace. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too13:50. Rychlé převinutí vpřed too15:00 toosee hello zásady nakonfigurované v editoru zásad hello, a potom too18:50 pro předvedení volání operace z portálu pro vývojáře hello s i bez hello vyžaduje autorizační token.  
  
```xml  
<!-- Copy hello following snippet into hello inbound section at hello api (or higher) level toopre-authorize access toooperations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Elementy  
  
|Element|Popis|Požaduje se|  
|-------------|-----------------|--------------|  
|ověřit token jwt|Kořenový element.|Ano|  
|cílové skupiny|Obsahuje seznam přijatelné cílovou skupinu deklarací identity, které mohou existovat na hello tokenu. Pokud nejsou více hodnot cílové skupiny, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje. Je třeba zadat alespoň jednu cílovou skupinu.|Ne|  
|Vystavitel podpisového klíče|Seznam kódování Base64 zabezpečení klíčů používaných toovalidate podepsané tokeny. Pokud nejsou více zabezpečení klíčů, pak se pokus o každý klíč dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje (je to užitečné pro výměnu tokenů). Mezi klíčové prvky mít volitelný `id` toomatch atribut použitý proti `kid` deklarací identity.|Ne|  
|vystavitele|Seznam přijatelné objektů, které vydán hello token. Pokud jsou přítomny více hodnot vystavitele, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje.|Ne|  
|Konfigurace openid|element Hello použit k určení kompatibilní endpoint konfigurace Open ID, ze kterého lze získat podpisového klíče a vystavitele.|Ne|  
|požadované deklarace identity|Obsahuje seznam deklarací identity očekává toobe na hello token k dispozici pro jeho toobe považován za platný. Když hello `match` atribut je nastaven příliš`all` každá hodnota deklarace identity v hello zásady musí být součástí hello token pro toosucceed ověření. Když hello `match` atribut je nastaven příliš`any` nejméně jedna deklarace identity musí být k dispozici v tokenu hello pro toosucceed ověření.|Ne|  
|záhlaví zumo hlavního klíče|Hlavní klíč pro tokeny vydané službou Azure Mobile Services|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|hodiny zkosení|Časový interval. Poskytuje některé malé volně mohou v případě, že deklarace identity vypršení platnosti tokenu hello je k dispozici v tokenu hello a následuje po hello aktuální datum a čas.|Ne|0 sekund|  
|se nezdařilo ověření chybových zpráv|Chybová zpráva tooreturn v textu odpovědi hello HTTP, pokud hello JWT nesplňuje podmínky ověření. Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.|Ne|Výchozí chybovou zprávu, závisí na ověření problém, například "JWT nejsou k dispozici."|  
|se nezdařilo httpcode ověření|Stav protokolu HTTP kód tooreturn Pokud hello JWT neprojde ověření.|Ne|401|  
|název hlavičky|Název Hello hello HTTP hlavičky, která uchovává hello token.|Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.|Není k dispozici|  
|id|Hello `id` atribut hello `key` element vám umožní toospecify hello řetězec, který bude odpovídat proti `kid` deklarací identity v tokenu (pokud existuje) toofind hello se hello odpovídající klíče toouse pro ověření podpisu.|Ne|Není k dispozici|  
|Shoda|Hello `match` atribut hello `claim` element určuje, zda každá hodnota deklarace identity v hello zásady musí být pro ověření toosucceed k dispozici v tokenu hello. Možné hodnoty:<br /><br /> -                          `all`-Každá hodnota deklarace identity v hello zásady musí být součástí hello token pro toosucceed ověření.<br /><br /> -                          `any`-Hodnota nejméně jedna deklarace identity musí být dostupná v tokenu hello toosucceed ověření.|Ne|Všechny|  
|Název dotazu paremeter|Hello název parametru dotazu hello hello podržíte hello token.|Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.|Není k dispozici|  
|požadovat čas vypršení platnosti|Logická hodnota. Určuje, jestli je v tokenu hello vyžaduje deklaraci identity vypršení platnosti.|Ne|Hodnota TRUE|
|vyžadovat schéma|název schématu hello tokenu, například Hello "Nosiče". Když tento atribut nastavený, zásady hello zajistí, který zadán, je k dispozici v hello hodnota hlavičky ověřování schématu.|Ne|Není k dispozici|
|Vyžadovat podepsané tokeny|Logická hodnota. Určuje, zda token je požadovaná toobe podepsané.|Ne|Hodnota TRUE|  
|Adresa URL|Otevřete adresu URL koncového bodu ID konfigurace ze které lze získat metadata Open ID konfigurace. Pro Azure Active Directory použít následující adresu URL hello: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` nahraďte název vašeho adresáře klienta, například `contoso.onmicrosoft.com`.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  
