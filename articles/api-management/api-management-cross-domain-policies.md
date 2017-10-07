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
# <a name="api-management-cross-domain-policies"></a>Zásady pro API Management napříč doménami
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CrossDomainPolicies"></a>Různé zásady domény  
  
-   [Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní hello rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.  
  
-   [CORS](api-management-cross-domain-policies.md#CORS) -přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.  
  
-   [JSONP](api-management-cross-domain-policies.md#JSONP) – přidá JSON s odsazení (JSONP) operaci tooan podpory nebo rozhraní API tooallow napříč doménami volá z klientů JavaScript založené na prohlížeči.  
  
##  <a name="AllowCrossDomainCalls"></a>Povolit volání mezi doménami  
 Použití hello `cross-domain` zásad toomake hello rozhraní API, které jsou přístupné z Adobe Flash a Microsoft Silverlight klienty založené na prohlížeči.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|mezi doménami|Kořenový element. Podřízené elementy musí odpovídat toohello [specifikaci souboru zásady mezi doménami Adobe](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Ano|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** globální  
  
##  <a name="CORS"></a>CORS  
 Hello `cors` zásad přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.  
  
 CORS umožňuje prohlížeče a server toointeract a zjistěte, zda tooallow konkrétní cross-origin požadavky (tj. XMLHttpRequests volání z jazyka JavaScript webové stránce tooother domény). To umožňuje více flexibility než povolíte jen žádostí stejné zdroji, ale je bezpečnější než povolení všech žádostí napříč zdroji.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
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
  
### <a name="example"></a>Příklad  
 Tento příklad ukazuje, jak před letu toosupport požadavky, například s vlastní hlavičky nebo jiných metod než GET a POST. vlastní hlavičky toosupport a další příkazy HTTP, používat hello `allowed-methods` a `allowed-headers` částech, jak ukazuje následující příklad hello.  
  
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
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|cors|Kořenový element.|Ano|Není k dispozici|  
|Povolené zdroje|Obsahuje `origin` prvky, které popisují hello povolené zdroje pro požadavky napříč doménami. `allowed-origins`může obsahovat buď jediný `origin` element, který určuje `*` tooallow jakýkoli původ nebo jeden nebo více `origin` elementy, které obsahují identifikátoru URI.|Ano|Není k dispozici|  
|Počátek|Hello hodnota může být buď `*` tooallow všechny původy nebo identifikátor URI, který určuje jeden počátek. Hello identifikátor URI musí obsahovat schéma, hostitele a portu.|Ano|Při vynechání je hello port v identifikátoru URI je používá port 80 pro protokol HTTP a port 443 se používá pro protokol HTTPS.|  
|povolené metody|Tento prvek je nutný, pokud metody jiné než GET nebo POST jsou povoleny. Obsahuje `method` elementy, které určují hello podporované příkazy HTTP.|Ne|Pokud v této části není zadán, jsou podporovány GET a POST.|  
|– Metoda|Určuje příkaz HTTP.|Alespoň jeden `method` prvek je nutný, pokud hello `allowed-methods` část je k dispozici.|Není k dispozici|  
|povolené hlavičky|Tento prvek obsahuje `header` elementy zadání názvy hello hlavičky, které můžou být součástí hello požadavku.|Ne|Není k dispozici|  
|Zveřejněte hlavičky|Tento prvek obsahuje `header` elementy zadání názvy hello hlavičky, které budou přístupné klientem hello.|Ne|Není k dispozici|  
|záhlaví|Určuje název hlavičky.|Alespoň jeden `header` element je vyžadována ve `allowed-headers` nebo `expose-headers` Pokud hello části nachází.|Není k dispozici|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Povolit přihlašovací údaje|Hello `Access-Control-Allow-Credentials` hlavičky v hello předběžných odpovědi bude nastavte toohello hodnotu tohoto atributu a ovlivnit hello klienta možnost toosubmit přihlašovací údaje v žádosti napříč doménami.|Ne|False|  
|Kontrola před výstupem výsledek max-age|Hello `Access-Control-Max-Age` hlavičky v hello předběžných odpovědi bude nastavte toohello hodnotu tohoto atributu a ovlivnit hello uživatelský agent možnost toocache před letu odpovědi.|Ne|0|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** rozhraní API, operace  
  
##  <a name="JSONP"></a>JSONP  
 Hello `jsonp` zásad přidá JSON s odsazení operace tooan podporu (JSONP) nebo volání rozhraní API tooallow napříč doménami z klientů JavaScript založené na prohlížeči. JSONP je metoda použitá při JavaScript programy toorequest data ze serveru v jiné doméně. JSONP obchází hello omezení vynucená většina webových prohlížečů, kde musí být tooweb stránek v hello stejné domény.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 Pokud jste volali metodu hello bez parametru zpětného volání hello? cb = XXX vrátí prostý JSON (bez obálku volání funkce).  
  
 Pokud přidáte parametr zpětného volání hello `?cb=XXX` vrátí výsledku JSONP zabalení hello původní výsledky JSON kolem funkce zpětného volání hello jako`XYZ('<json result goes here>');`  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|JSONP|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Název parametru zpětného volání|Hello volání funkce JavaScript napříč doménami s předponou hello plně kvalifikovaný název domény, kde hello funkce nachází.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** odchozí  
  
-   **Zásady obory:** globální, produktu, rozhraní API, operace  
  
## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  