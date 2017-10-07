---
title: "aaaCustom ukládání do mezipaměti ve službě Azure API Management"
description: "Zjistěte, jak toocache položky podle klíče ve službě Azure API Management"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 681380743c8c96af5d0a8e25948a8c0663e9fd35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-caching-in-azure-api-management"></a>Vlastní ukládání do mezipaměti ve službě Azure API Management
Služba Azure API Management má integrovanou podporu pro [ukládání do mezipaměti odpovědi HTTP](api-management-howto-cache.md) pomocí adresy URL prostředku hello jako klíč hello. Hello klíč mohou být upravena hlavičky žádosti pomocí hello `vary-by` vlastnosti. To je užitečné pro ukládání do mezipaměti celé odpovědi protokolu HTTP (neboli reprezentace), ale někdy je užitečné toojust mezipaměti část reprezentace. Hello nové [hodnota mezipaměti vyhledávání](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) a [hodnota úložiště mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) zásady poskytují možnost hello toostore a načtení libovolný kusy data přímo z definice zásady. Tato možnost také přidá hodnotu toohello dříve zavedená [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad vzhledem k tomu, že teď můžete mezipaměti odpovědí z externích služeb.

## <a name="architecture"></a>Architektura
Služba API Management používá datové mezipaměti sdíleného za klienta tak, aby při změně měřítka toomultiple jednotky budete stále dostávat přístup toohello stejná data jako do mezipaměti. Při práci s více oblast nasazení existují však nezávislé mezipaměti v každém z oblasti hello. Kvůli toothis, je důležité toonot považovat hello mezipaměti jako úložiště dat, kde je jediný zdroj hello některé část informace. Pokud nebyla a později se rozhodli tootake výhod nasazení s více oblast hello, zákazníkům, kteří cestují, může ztratit přístup k datům toothat do mezipaměti.

## <a name="fragment-caching"></a>Fragment ukládání do mezipaměti
Existují určité případy, kde odpovědí nevrátila obsahovat některé část dat, která je nákladná toodetermine a ještě stále aktuální pro přiměřené době. Jako příklad zvažte službu vytvořené letecká společnost, která obsahuje informace týkající se rezervace letu, stav letu, atd. Pokud uživatel hello je členem programu hello airlines body, mají by také informace týkající se aktuální stav tootheir a vzdálenost nahromadění. Tyto informace související s uživatelem se pravděpodobně uloží do jiného systému, ale může být žádoucí tooinclude v odpovědi vrátí o letu stav a rezervace. To lze provést pomocí procesu nazývaného fragment ukládání do mezipaměti. primární reprezentace Hello lze vrátit ze serveru počátek hello pomocí nějaký druh tokenu tooindicate, kde informace týkající se uživatele hello je toobe vložit. 

Vezměte v úvahu hello následující JSON odpověď z back-end rozhraní API.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

A sekundární prostředek na úrovni `/userprofile/{userid}` jako,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

V pořadí toodetermine hello odpovídající uživatelské informace tooinclude potřebujeme tooidentify, který je hello koncového uživatele. Tento mechanismus je závisí na implementaci. Jako příklad používám hello `Subject` z deklarací identity `JWT` tokenu. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

Nemůžeme Uložit `enduserid` hodnotu v kontextu proměnné pro pozdější použití. dalším krokem Hello je toodetermine, pokud už na předchozí požadavek načíst informace o uživateli hello a uložené v mezipaměti hello. Pro tento používáme hello `cache-lookup-value` zásad.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Pokud neexistuje žádná položka v mezipaměti hello, která odpovídá hodnota klíče toohello a ne `userprofile` vytvoří kontextové proměnné. Ověření, zda hello úspěšné vyhledání hello pomocí hello `choose` řízení toku zásad.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

Pokud hello `userprofile` kontextové proměnné neexistuje, pak jsme tyto probíhající toohave toomake protokolu HTTP žádosti tooretrieve ho.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points toohello profile for hello current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

Používáme hello `enduserid` tooconstruct hello URL toohello uživatelského profilu prostředku. Jakmile jsme hello odpovědi, jsme hello základní text z odpovědi hello pro vyžádání obsahu a uloží jej zpět do kontextové proměnné.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

tooavoid nám s toomake tento požadavek HTTP znovu, pokud hello stejný uživatel zadá další žádost, lze uložit profil uživatele hello v mezipaměti hello.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Uložíme hello hodnota v mezipaměti hello pomocí hello přesně stejný klíč jsme, že původně pokusil tooretrieve se službou. Hello dobu, po kterou vybereme možnost toostore hello hodnota by měla být založena na tom, jak často hello informace o změny a jak odolný vůči chybám uživatelé jsou tooout informace o datu. 

Je důležité toorealize že načítání z mezipaměti hello je stále mimo proces, požadavku sítě a potenciálně můžete přesto přidat desítkami milisekundách toohello požadavku. výhody Hello pocházet pokud trvá výrazně delší, než je z důvodu tooneeding toodo databázové dotazy nebo agregační informace z více back EndY určení hello informace o uživatelském profilu.

Hello posledním krokem v procesu hello je tooupdate hello vrátí odpověď se naše informace o profilu uživatele.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Vybrali jste tooinclude hello uvozovek jako součást tokenu hello tak, že i když nedojde, nahraďte text hello, hello odpověď byla stále platný kód JSON. To se primárně toomake snazší ladění.

Jakmile se společně sloučit všechny tyto kroky, je hello konečný výsledek zásadu, která vypadá jako hello následující jeden.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in hello cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in hello cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request tooget user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points toohello profile for hello current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Tento přístup ukládání do mezipaměti se primárně používá v weby kde HTML tvoří na straně serveru hello tak, aby je možné vykreslit jako jednu stránku. Však může také být užitečné v rozhraní API, kde klienti nemohou provádět klienta na straně ukládání do mezipaměti protokolu HTTP nebo je žádoucí není tooput tuto odpovědnost hello klienta.

Tento stejný druh ukládání do mezipaměti fragment provést také v hello back-end webových serverů pomocí ukládání do mezipaměti serveru Redis, ale pomocí tooperform služby API Management hello tato práce je užitečné, když hello mezipaměti fragmenty pocházejí z různých back EndY než hello primární odpovědi.

## <a name="transparent-versioning"></a>Transparentní Správa verzí
Je obvyklé, více verzí jinou implementaci rozhraní API toobe podporované v daném okamžiku. Toto je možná toosupport různých prostředích, jako je vývojářů, testovací, produkční atd., nebo může být toosupport starší verze času toogive hello rozhraní API pro rozhraní API příjemci toomigrate toonewer verze. 

Jeden ze způsobů toohandling tomto místo z nutnosti klienta vývojáři toochange hello adresy z `/v1/customers` příliš`/v2/customers` je toostore v hello příjemce data profilu, kterou verzi hello rozhraní API se v současné době chcete toouse a volání hello odpovídající Adresa URL back-end. V aplikaci pořadí toodetermine hello správné back-end adresy URL toocall pro konkrétního klienta, je nutné tooquery některé konfigurační data. Pomocí ukládání do mezipaměti tato konfigurační data, jsme minimalizovat snížení výkonu hello to toto vyhledávání.

prvním krokem Hello je, že identifikátor toodetermine hello používá tooconfigure hello požadovanou verzi. V tomto příkladu vybrali jste klíč předplatného tooassociate hello verze toohello produktu. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

Toosee vyhledávání mezipaměti jsme pak dělat, když jsme již byla načtena verze hello požadované klienta.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Potom jsme zkontrolujte toosee, pokud jsme ho nebyl nalezen v mezipaměti hello.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Pokud jsme nebyla pak jsme přejděte a jejich načtení.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Extrahuje hello odpovědi základní text z odpovědi hello.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Uložte ji zpět do hello mezipaměti pro budoucí použití.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

A nakonec hello back-end adresy URL tooselect hello verze aktualizace požadované klientem hello hello služby.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

Hello úplně zásady je následující.

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in hello cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

Povolení rozhraní API příjemci tootransparently řízení které back-end verze je aktuálně používán klientů bez nutnosti tooupdate a znovu ho zaveďte klientů je elegantní řešení, které řeší mnoho aspekty správy verzí rozhraní API.

## <a name="tenant-isolation"></a>Izolaci klientů
U rozsáhlejších, víceklientské nasazení některé společnosti vytvořit samostatné skupiny klientů na odlišné nasazení hardwaru back-end. Tím se minimalizují počet hello zákazníci, kteří jsou ovlivněný problémem hardwaru na back-end hello. Umožňuje také nové toobe verze softwaru ve fázích. Tato architektura back-end v ideálním případě by měl být transparentní tooAPI příjemci. Toho lze dosáhnout v podobně jako verze tootransparent způsob aplikace je založena na hello stejným způsobem manipulace s hello URL back-end pomocí konfigurace stav pro každý klíč rozhraní API.  

Místo vrácení upřednostňované verzi hello rozhraní API pro každý klíč předplatného, by vrátit identifikátor, který má vztah skupinu klienta toohello přiřazené hardwaru. Tento identifikátor mohou být použité tooconstruct hello odpovídající back-end adresy URL.

## <a name="summary"></a>Souhrn
Hello volnost toouse hello Azure API management mezipaměti pro ukládání libovolného typu dat umožňuje efektivní přístup tooconfiguration data, která může mít vliv na způsob hello zpracování příchozí žádosti. Lze také použít toostore fragmenty dat, které můžete posílení odpovědi, vrácená z rozhraní API back-end.

## <a name="next-steps"></a>Další kroky
Prosím sdělte svůj názor v hello vlákna služby Disqus pro toto téma případné další scénáře, tyto zásady mají povolený pro vás, nebo pokud existují scénáře byste chtěli tooachieve, ale není zaregistrované, jsou aktuálně možné.

