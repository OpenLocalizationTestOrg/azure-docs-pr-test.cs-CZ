---
title: "aaaUsing žádosti o toogenerate HTTP služby API Management"
description: "Další informace toouse žádostí a odpovědí zásad v API Management toocall externích služeb z rozhraní API"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 8002ee453057513340328d99f298703c3b3a9531
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-external-services-from-hello-azure-api-management-service"></a>Pomocí externích služeb z hello služby Azure API Management
Hello zásady, které jsou dostupné ve službě Azure API Management service může provádět široké škály užitečné pracovní výhradně na základě příchozího požadavku hello, hello odchozí odpovědi a informace o základní konfiguraci. Ale je možné toointeract pomocí externích služeb z rozhraní API správy zásad mnoho většího počtu možností otevře.

Jsme předtím viděli, jak jsme mohou komunikovat s hello [centra událostí Azure služby pro protokolování, sledování a analýza](api-management-log-to-eventhub-sample.md). V tomto článku jsme ukazují, že zásady, které vám umožňují toointeract s žádné externí HTTP na základě služby. Tyto zásady lze pro spuštění vzdáleného události nebo pro načtení informací, které budou použité toomanipulate hello původní žádosti a odpovědi nějakým způsobem.

## <a name="send-one-way-request"></a>Odeslání jeden způsob požadavků
Pravděpodobně hello nejjednodušší externí interakce je hello ještě efektivněji a zapomněli styl požadavek, který umožňuje externí služba toobe oznámení určitého druhu důležité události. Můžeme použít zásady toku řízení hello `choose` toodetect jakýkoli druh podmínku, kterou jsme zajímá a poté, pokud hello podmínky splněny, abychom mohli provádět externí požadavku HTTP pomocí hello [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) zásad. To může být žádost tooa, zasílání zpráv systému jako Hipchat nebo Slack nebo pošty rozhraní API jako sendgrid vám umožňuje nebo MailChimp, nebo pro kritické podpory incidentů něco jako PagerDuty. Všechny tyto systémy zasílání zpráv mají jednoduché rozhraní API HTTP, který lze snadno vyvolat.

### <a name="alerting-with-slack"></a>Výstrahy s Slack
Hello následující příklad ukazuje, jak toosend tooa zpráva Slack chatovací místnosti, pokud je stavový kód odpovědi HTTP hello větší než nebo roven hodnotě too500. Rozsah 500 Chyba označuje, že k problému s naše back-end rozhraní API, která hello klienta našem rozhraní API nelze vyřešit sami. Obvykle vyžaduje nějaký druh zásahu na našem část.  

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

Slack má hello představu o háky příchozí webové. Při konfiguraci příchozí webové háku, generuje Slack speciální adresy URL, která vám umožní do kanálu Slack hello toodo jednoduché požadavek POST a toopass zprávu. text JSON, který vytváříme Hello je založena na formátu definované Slack.

![Slack webové háku](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Je ještě efektivněji a zapomněli dostatečně vhodné?
Existují určité kompromisy při použití styl ještě efektivněji a zapomněli požadavku. Pokud z jakéhokoliv důvodu hello požadavek selže a pak nebudou oznámeny hello selhání. V takovém případě konkrétní není oprávněná hello složitost systému hlášení selhání sekundárního objektu a hello další výkonová náročnost čekání na odezvu hello. Pro scénáře, kdy je zásadní toocheck hello odpovědi, pak hello [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad je lepší volbou.

## <a name="send-request"></a>Odeslání žádosti
Hello `send-request` zásad umožňuje pomocí tooperform externí služba, která je komplexní zpracování funkce a služba správy toohello rozhraní API vracená data, která lze použít pro další zpracování zásad.

### <a name="authorizing-reference-tokens"></a>Autorizace tokeny odkazů
Hlavní funkce API Management chrání back-endové prostředky. Pokud serveru ověřování hello používá vaše rozhraní API vytvoří [tokeny JWT](http://jwt.io/) jako součást jeho tok OAuth2, jako [Azure Active Directory](../active-directory/active-directory-aadconnect.md) nemá, můžete použít hello `validate-jwt` platnost hello tooverify zásad Hello token. Ale některé servery autorizace vytvořit, co se nazývají [odkazovat tokeny](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , nelze ověřit bez provedení serveru autorizace toohello zpětné volání.

### <a name="standardized-introspection"></a>Standardizovaná introspection
V posledních hello byl nijak standardizované ověřit token odkazu se serverem ověřování. Ale nedávno navrhovaný standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publikoval hello IETF, která definuje, jak server prostředků můžete ověřit hello platnost tokenu.

### <a name="extracting-hello-token"></a>Extrahování hello tokenu
prvním krokem Hello je tooextract hello tokenu z hello autorizační hlavičky. Hodnota hlavičky Hello musí být naformátovaná hello `Bearer` autorizace schématu, jedna mezera a pak hello autorizační token dle [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Bohužel existují případech, kdy je vynechán hello autorizace schéma. tooaccount to při analýze, jsme rozdělit hello hodnota záhlaví na místo a vyberte hello poslední řetězec z hello vrátila pole řetězců. To poskytuje řešení chybně formátovaný autorizační hlavičky.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a>Vytváření hello ověření požadavku
Jakmile jsme hello autorizační token, abychom mohli provádět hello požadavek toovalidate hello token. RFC 7662 volá tento proces introspection a vyžaduje, aby vám `POST` prostředku introspection toohello formuláře HTML. Hello formuláře HTML musí obsahovat alespoň dvojici klíč/hodnota s klíčem hello `token`. Tento požadavek toohello autorizace server musí být také ověřené tooensure, který nelze přejít vlečným škodlivý klientů pro platné tokeny.

```xml
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
```

### <a name="checking-hello-response"></a>Kontrola, zda text hello odpovědi
Hello `response-variable-name` atribut je použité toogive přístup hello vrátil odpověď. Hello název definovaný v této vlastnosti lze použít jako klíč do hello `context.Variables` slovník tooaccess hello `IResponse` objektu.

Z objektu odpovědi hello načteme textu hello a RFC 7622 víme, že hello odpovědi musí být objekt JSON a musí obsahovat aspoň vlastnost s názvem `active` tedy logickou hodnotu. Když `active` má hodnotu true, pak hello token je považován za platný.

### <a name="reporting-failure"></a>Generování sestav selhání
Používáme `<choose>` zásad toodetect Pokud hello token je neplatný a pokud ano, vrátí odpovědi 401.

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

Dle [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) který popisuje, jak `bearer` slouží tokeny, také vrátíme `WWW-Authenticate` hlavičky odpovědi hello 401. Hello WWW-Authenticate je určený tooinstruct klienta o tooconstruct žádost správně autorizované. Z důvodu toohello širokou škálu možných s hello OAuth2 framework přístupy, je obtížné toocommunicate hello všechny potřebné informace. Naštěstí existuje úsilí zpracováváme toohelp [klienti zjišťovat, jak tooproperly autorizaci požadavků tooa prostředků serveru](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Konečné řešení
Všechny součásti hello uvedení společně, se nám získat hello následující zásady:

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

Toto je pouze jeden z mnoha příklady, jak hello `send-request` zásad může být užitečné externích služeb používaných toointegrate do procesu hello požadavků a odpovědí prostřednictvím hello služba API Management.

## <a name="response-composition"></a>Složení odpovědi
Hello `send-request` zásadu lze použít pro rozšíření systému primární požadavek tooa back-end, jako jsme viděli v předchozím příkladu hello, nebo můžete využít jako kompletní nahrazení pro volání hello back-end. Touto technikou jsme můžete snadno vytvořit složené prostředky, které jsou seskupeny z několika různými systémy.

### <a name="building-a-dashboard"></a>Vytváření řídicího panelu
Někdy budete chtít toobe možné tooexpose informace, které existuje ve více back-end systémy, například toodrive řídicí panel. Hello klíčových ukazatelů výkonu pocházet ze všech různých back EndY, ale si přejete není toothem tooprovide přímý přístup a bude dobrý, v jedné žádosti může načíst všechny informace o hello. Možná některé z informací hello back-end vyžaduje některé segmentování a analyzování a úpravě trochu nejprve! Je možné toocache že složené prostředků budou užitečné tooreduce back-end hello načíst jako víte, že uživatelé mají která podporují ražením klávesu F5 hello v toosee pořadí, pokud jejich underperforming metriky se může změnit.    

### <a name="faking-hello-resource"></a>Faking hello prostředků
první krok toobuilding Hello naše prostředek řídicí panel je tooconfigure operaci nového portálu vydavatele hello API Management. Toto bude naše zásady toobuild složení tooconfigure operace použité zástupný symbol naše dynamické prostředků.

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a>Provádění požadavků hello
Jednou hello `dashboard` operace byla vytvořena jsme můžete nakonfigurovat zásadu speciálně pro tuto operaci. 

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

prvním krokem Hello je tooextract žádné parametry dotazu z příchozího požadavku hello, takže jsme může předávat tooour back-end. V tomto příkladu se naše řídicí panel zobrazuje informace založené na určitou dobu tedy `fromDate` a `toDate` parametr. Můžeme použít hello `set-variable` informace o zásadách tooextract hello z adresy URL žádosti hello.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Jakmile tyto informace můžeme provádět požadavky tooall hello back-end systémy. Každý požadavek vytvoří novou adresu URL s informace o parametrech hello volá jeho příslušný server a ukládá hello odpovědi v kontextové proměnné.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Tyto požadavky bude spuštěn v pořadí, což není ideální. V budoucích vydání jsme zavádí nové zásady názvem `wait` , povolí všechny tyto požadavky tooexecute paralelně.

### <a name="responding"></a>Neodpovídá
tooconstruct hello složené odpovědi používáme hello [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásad. Hello `set-body` element můžete použít výrazu tooconstruct nový `JObject` s všechny součásti reprezentace hello vložených jako vlastnosti.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

dokončení zásad Hello vypadá takto:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

V konfiguraci hello hello zástupný symbol operace, které jsme můžete konfigurovat hello řídicí panel prostředků toobe uložená v mezipaměti pro alespoň jednu hodinu vzhledem k tomu, že jsme srozumitelná hello povaha hello dat znamená, že i když je zastaralý, že budou mít pořád dostatečně efektivní hodinu Uživatelé toohello tooconvey cenné informace.

## <a name="summary"></a>Souhrn
Služba Azure API Management nabízí flexibilní zásady, které je možné selektivně použít tooHTTP provoz a umožňuje složení služeb back-end. Jestli chcete tooenhance bránu rozhraní API s výstrahy funkce, ověření, možnosti ověření nebo vytvořit nové složené prostředky v závislosti na více služeb back-end, hello `send-request` a související zásady otevřete široké možnosti.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video s přehledem těchto zásad
Další informace o hello [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), a [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásady popsaná v tomto článku, podívejte se prosím hello následující video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

