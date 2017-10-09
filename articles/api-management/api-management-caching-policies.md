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
# <a name="api-management-caching-policies"></a>Ukládání do mezipaměti zásady služby API Management
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a>Zásady ukládání do mezipaměti  
  
-   Odpověď zásady ukládání do mezipaměti  
  
    -   [Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou do mezipaměti odpovědi, pokud je k dispozici.  
  
    -   [Uložení toocache](api-management-caching-policies.md#StoreToCache) – mezipamětí odpovědí podle konfigurace toohello zadaný mezipaměti ovládacího prvku.  
  
-   Hodnota zásady ukládání do mezipaměti  
  
    -   [Získat hodnotu z mezipaměti](#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.  
  
    -   [Uložení v mezipaměti hodnoty](#StoreToCacheByKey) -uložit položku hello mezipaměti podle klíče.  
  
    -   [Odebrat hodnotu z mezipaměti](#RemoveCacheByKey) -odebrání položky v mezipaměti hello podle klíče.  
  
##  <a name="GetFromCache"></a>Získat z mezipaměti  
 Použití hello `cache-lookup` tooperform mezipaměť zásad vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici. Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu. Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na hello back-end webový server a snižuje latence, zaznamenatelného spotřebiteli rozhraní API.  
  
> [!NOTE]
>  Tato zásada musí mít odpovídající [toocache úložiště](api-management-caching-policies.md#StoreToCache) zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
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
  
### <a name="examples"></a>Příklady  
  
#### <a name="example"></a>Příklad  
  
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
  
#### <a name="example-using-policy-expressions"></a>Příklad pomocí výrazů zásad  
 Tento příklad ukazuje, jak odpovědi API Management tootooconfigure ukládání do mezipaměti Doba trvání, zda text hello odpovídá ukládání odpovědí do mezipaměti hello back-end služby podle specifikace hello zálohovaný služby `Cache-Control` – direktiva. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too25:25.  
  
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
  
 Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|vyhledávání v mezipaměti|Kořenový element.|Ano|  
|měnit podle hlavičky|Spustit ukládání do mezipaměti odpovědi na hodnotu zadaného záhlaví, jako je například přijmout, Accept-Charset, Accept-Encoding, Accept-Language, autorizace, Expect, z hostitele, If-Match.|Ne|  
|lišit podle--parametr dotazu|Spusťte ukládání do mezipaměti odpovědi na hodnotu zadanými parametry dotazu. Zadejte jeden nebo více parametrů. Použijte středník jako oddělovač. Pokud se zadaný žádný, se používají všechny parametry dotazu.|Ne|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Povolit privátní odpovědi-ukládání do mezipaměti|Pokud nastavíte příliš`true`, umožňuje ukládání do mezipaměti požadavků, které obsahují autorizační hlavičky.|Ne|False|  
|podřízený typ ukládání do mezipaměti|Tento atribut musí být nastavena tooone hello následující hodnoty.<br /><br /> -none - podřízené ukládání do mezipaměti není povolen.<br />-soukromý – podřízené privátní ukládání do mezipaměti je povolen.<br />-veřejný - privátní a sdílené podřízené ukládání do mezipaměti je povolen.|Ne|None|  
|musí revalidate|Pokud je povoleno ukládání do mezipaměti podřízené tento atribut Zapne nebo vypne hello `must-revalidate` direktiva ovládací prvek mezipaměti v odpovědi brány.|Ne|Hodnota TRUE|  
|se liší podle vývojáře|Nastavení příliš`true` toocache odpovědi na vývojáře klíč.|Ne|False|  
|se liší podle vývojáře skupiny –|Nastavení příliš`true` toocache odpovědi na roli uživatele.|Ne|False|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** rozhraní API, operace, produktu  
  
##  <a name="StoreToCache"></a>Toocache úložiště  
 Hello `cache-store` odpovědí mezipamětí zásady podle toohello zadaná nastavení mezipaměti. Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu. Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na hello back-end webový server a snižuje latence, zaznamenatelného spotřebiteli rozhraní API.  
  
> [!NOTE]
>  Tato zásada musí mít odpovídající [získat z mezipaměti](api-management-caching-policies.md#GetFromCache) zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Příklady  
  
#### <a name="example"></a>Příklad  
  
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
  
#### <a name="example-using-policy-expressions"></a>Příklad pomocí výrazů zásad  
 Tento příklad ukazuje, jak odpovědi API Management tootooconfigure ukládání do mezipaměti Doba trvání, zda text hello odpovídá ukládání odpovědí do mezipaměti hello back-end služby podle specifikace hello zálohovaný služby `Cache-Control` – direktiva. Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too25:25.  
  
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
  
 Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|úložiště mezipaměti|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Doba trvání|Time-to-live Dobrý den do mezipaměti položky, které jsou zadány v sekundách.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** odchozí  
  
-   **Zásady obory:** rozhraní API, operace, produktu  
  
##  <a name="GetFromCacheByKey"></a>Získat hodnotu z mezipaměti  
 Použití hello `cache-lookup-value` tooperform zásady mezipaměti vyhledávání pomocí klíče a vrátí hodnotu uloženou v mezipaměti. klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.  
  
> [!NOTE]
>  Tato zásada musí mít odpovídající [uložení hodnoty v mezipaměti](#StoreToCacheByKey) zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Příklad  
 Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|hodnota mezipaměti vyhledávání|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Výchozí hodnota|Hodnota, která se přiřadí proměnné toohello Pokud vyhledávání klíčů mezipaměti hello vedlo k neúspěšnému přístupu. Pokud se tento atribut nezadá, `null` je přiřazen.|Ne|`null`|  
|key|Hodnota klíče toouse hello vyhledávání v mezipaměti.|Ano|Není k dispozici|  
|Název proměnné|Název hello [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) hello prohledávat hodnotu budou přiřazeny, pokud je úspěšné vyhledávání. Pokud vyhledávání vede k neúspěšnému přístupu do, hello proměnnou, bude přiřazen hello hodnotu hello `default-value` atribut nebo `null`, pokud hello `default-value` atribut vynechán.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** globální, rozhraní API, operace, produktu  
  
##  <a name="StoreToCacheByKey"></a>Hodnota úložiště v mezipaměti  
 Hello `cache-store-value` provede úložiště mezipaměti podle klíče. klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.  
  
> [!NOTE]
>  Tato zásada musí mít odpovídající [získat hodnotu z mezipaměti](#GetFromCacheByKey) zásad.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a>Příklad  
 Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|Hodnota úložiště mezipaměti|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Doba trvání|Hodnota bude do mezipaměti pro hello zadat hodnotu doby trvání, zadaný v sekundách.|Ano|Není k dispozici|  
|key|Hodnota klíče hello mezipaměti bude uložena v části.|Ano|Není k dispozici|  
|hodnota|toobe hodnota Hello do mezipaměti.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** globální, rozhraní API, operace, produktu  
  
###  <a name="RemoveCacheByKey"></a>Odebrat hodnotu z mezipaměti  
 Hello `cache-remove-value` odstraní identifikovaný svůj klíč položky v mezipaměti. klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.  
  
#### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Příklad  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|hodnota mezipaměti odebrat|Kořenový element.|Ano|  
  
#### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|key|klíč Hello hello dříve do mezipaměti hodnotu toobe odebraných z mezipaměti hello.|Ano|Není k dispozici|  
  
#### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Části zásady:** vstupní, výstupní a back-end, při chybě  
  
-   **Zásady obory:** globální, rozhraní API, operace, produktu  
  

## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  