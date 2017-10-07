---
title: "aaaError zpracování v zásad Azure API managementu | Microsoft Docs"
description: "Zjistěte, jak hello toorespond tooerror podmínky, které mohou nastat během zpracování požadavků ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 002c65f21e763fd644da61b6a11685ffd97488c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-api-management-policies"></a>Zpracování chyb v zásady služby API Management
Azure API Management umožňuje vydavatelů toorespond tooerror podmínky, které mohou nastat během zpracování hello požadavky toohello proxy tím, že poskytuje `ProxyError` objektu. Hello `ProxyError` objektu je přístupné přes hello [kontextu. Poslední chyba](api-management-policy-expressions.md#ContextVariables) vlastnost a mohou být využívána zásad v hello `on-error` zásad. Toto téma obsahuje odkaz na hello došlo k chybě funkce zpracování v Azure API Management.  
  
## <a name="error-handling-in-api-management"></a>Zpracování chyb v API Management  
 Zásady ve službě Azure API Management jsou rozdělené do `inbound`, `backend`, `outbound`, a `on-error` částech, jak ukazuje následující příklad hello.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements toobe applied toohello request go here -->  
  </inbound>  
  <backend>  
    <!-- statements toobe applied before hello request is   
         forwarded toohello backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements toobe applied toohello response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements toobe applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
 Během zpracování požadavku hello provádění předdefinované kroků spolu se všechny zásady, které jsou v oboru pro požadavek hello. Pokud dojde k chybě zpracování okamžitě skáče toohello `on-error` zásad. Hello `on-error` zásad můžete použít na jakémkoli oboru a vydavatelé rozhraní API můžete nakonfigurovat vlastní chování, například protokolování hello chyba tooevent centra nebo vytvořit nový volající toohello tooreturn odpovědi.  
  
> [!NOTE]
>  Hello `on-error` části není k dispozici v zásadách ve výchozím nastavení. tooadd hello `on-error` tooa zásady, procházet toohello požadovaných zásad v editoru zásad hello a přidejte ji. Další informace o konfiguraci zásad najdete v tématu [zásady ve službě API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Pokud není žádná `on-error` části volající bude přijímat zprávy odpovědi HTTP 400 nebo 500, pokud dojde k chybě.  
  
### <a name="policies-allowed-in-on-error"></a>Zásady povolené na chybu  
 Hello tyto zásady mohou být používány hello `on-error` zásad.  
  
-   [Zvolte](api-management-advanced-policies.md#choose)  
  
-   [nastavená proměnná](api-management-advanced-policies.md#set-variable)  
  
-   [hledání a nahrazování](api-management-transformation-policies.md#Findandreplacestringinbody)  
  
-   [vrátí odpověď](api-management-advanced-policies.md#ReturnResponse)  
  
-   [set – hlavička](api-management-transformation-policies.md#SetHTTPheader)  
  
-   [set – metoda](api-management-advanced-policies.md#SetRequestMethod)  
  
-   [Nastavit stav](api-management-advanced-policies.md#SetStatus)  
  
-   [odeslání žádosti](api-management-advanced-policies.md#SendRequest)  
  
-   [odeslání jeden způsob požadavků](api-management-advanced-policies.md#SendOneWayRequest)  
  
-   [protokol eventhub](api-management-advanced-policies.md#log-to-eventhub)  
  
-   [JSON do xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
  
-   [XML – json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>Poslední chyba  
 Když dojde k chybě a řízení skáče toohello `on-error` část zásad chyba hello je uložen v [kontextu. Poslední chyba](api-management-policy-expressions.md#ContextVariables) vlastnost, která je přístupná pomocí zásad v hello `on-error` části a má následující vlastnosti hello.  
  
|Name (Název)|Typ|Popis|Požaduje se|  
|----------|----------|-----------------|--------------|  
|Zdroj|Řetězec|Názvy hello elementu, kde došlo k chybě hello. Může být zásady nebo název kroku předdefinované kanálu.|Ano|  
|Důvod|Řetězec|Kód chyby popisný počítače, které by mohly být použity v zpracování chyb.|Ne|  
|Zpráva|Řetězec|Popis chyby čitelná pro člověka.|Ano|  
|Rozsah|Řetězec|Název oboru hello kde hello chyby došlo k chybě a může mít jednu z "globální", "produkt", "api" nebo "operace"|Ne|  
|Část|Řetězec|Název oddílu, kde došlo k chybě a může jednu z "příchozí", "back-end", "odchozí" nebo "na chyba".|Ne|  
|Cesta|Řetězec|Určuje vnořené zásad, například "[3] zvolte / při [2]".|Ne|  
|PolicyId|Řetězec|Hodnota hello `id` atribut, pokud zadaný zákazníkem hello na hello zásady, kde došlo k chybě|Ne|  
  
> [!NOTE]
>  Všechny zásady mít volitelný `id` atribut, který lze přidat toohello kořenový element hello zásad. Pokud se tento atribut nachází v zásadách, když dojde k chybě, můžete ji načíst hello hodnotu atributu hello pomocí hello `context.LastError.PolicyId` vlastnost.  
  
## <a name="predefined-errors-for-built-in-steps"></a>Předdefinované chyby pro vestavěné kroky  
 Hello tyto chyby jsou předdefinovány pro chybové podmínky, které se můžou vyskytnout během vyhodnocení hello předdefinované zpracování kroků.  
  
|Zdroj|Podmínka|Důvod|Zpráva|  
|------------|---------------|------------|-------------|  
|konfigurace|Identifikátor URI neodpovídá tooany rozhraní Api nebo operace|OperationNotFound|Nelze toomatch příchozí požadavek tooan operace.|  
|Autorizace|Není zadaný klíč předplatného|SubscriptionKeyNotFound|Přístup byl odepřen z důvodu toomissing klíč předplatného. Ujistěte se, že klíč předplatného tooinclude při provádění požadavků toothis rozhraní API.|  
|Autorizace|Hodnota klíče předplatného je neplatná.|SubscriptionKeyInvalid|Přístup byl odepřen z důvodu tooinvalid klíč předplatného. Ujistěte se, že tooprovide platný klíč pro aktivní předplatné.|  
  
## <a name="predefined-errors-for-policies"></a>Předdefinované chyby pro zásady  
 Hello tyto chyby jsou předdefinovány pro chybové podmínky, které se můžou vyskytnout během hodnocení zásad.  
  
|Zdroj|Podmínka|Důvod|Zpráva|  
|------------|---------------|------------|-------------|  
|omezení četnosti|Byl překročen limit rychlost|RateLimitExceeded|Překročení omezení četnosti|  
|kvóta|Kvóta byla překročena.|QuotaExceeded|Překročení kvóty volání. Kvóta bude doplnit v xx:xx:xx. - nebo - Out šířky pásma kvóty. Kvóta bude doplnit v xx:xx:xx.|  
|JSONP|Hodnota parametru zpětného volání je neplatný (obsahuje nesprávné znaky)|CallbackParameterInvalid|Hodnota parametru zpětného volání {zpětného volání název parametru} není platný identifikátor jazyka JavaScript.|  
|Filtr IP|Neúspěšné tooparse volající IP z požadavku|FailedToParseCallerIP|Nepodařilo tooestablish IP adresu pro volající hello. Přístup byl odepřen.|  
|Filtr IP|Volající IP není v seznamu povolených aplikací|CallerIpNotAllowed|Volající IP adresa {adresa} není povolena. Přístup byl odepřen.|  
|Filtr IP|Volající IP je v seznamu blokovaných položek|CallerIpBlocked|Zablokování volající IP adresy. Přístup byl odepřen.|  
|Kontrola – hlavička|Chybí požadovaná hlavička nebyla uvedené nebo hodnota|HeaderNotFound|Záhlaví {název hlavičky} nebyl nalezen v žádosti o hello. Přístup byl odepřen.|  
|Kontrola – hlavička|Chybí požadovaná hlavička nebyla uvedené nebo hodnota|HeaderValueNotAllowed|Hodnota hlavičky {název hlavičky} {hodnoty hlavičky} není povolená. Přístup byl odepřen.|  
|ověřit token jwt|V požadavku chybí Jwt token|TokenNotFound|Nebyl nalezen v žádosti o hello JWT. Přístup byl odepřen.|  
|ověřit token jwt|Nepovedlo se ověřit podpis|TokenSignatureInvalid|< zprávy z knihovny jwt\>. Přístup byl odepřen.|  
|ověřit token jwt|Cílová skupina je neplatná|TokenAudienceNotAllowed|< zprávy z knihovny jwt\>. Přístup byl odepřen.|  
|ověřit token jwt|Neplatný vystavitele|TokenIssuerNotAllowed|< zprávy z knihovny jwt\>. Přístup byl odepřen.|  
|ověřit token jwt|Platnost tokenu|TokenExpired|< zprávy z knihovny jwt\>. Přístup byl odepřen.|  
|ověřit token jwt|Klíč podpisu nebyla rozpoznána podle id|TokenSignatureKeyNotFound|< zprávy z knihovny jwt\>. Přístup byl odepřen.|  
|ověřit token jwt|Chybí požadovaná deklarací z tokenu|TokenClaimNotFound|JWT token chybí hello následující deklarace identity: < c1\>, < c2\>,... Přístup byl odepřen.|  
|ověřit token jwt|Neshoda hodnoty deklarace identity|TokenClaimValueNotAllowed|Hodnota deklarace identity {deklarace identity name} {hodnota deklarace} není povolená. Přístup byl odepřen.|  
|ověřit token jwt|Jiné chyby ověření|JwtInvalid|< zprávy z knihovny jwt\>|

## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  