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
# <a name="using-external-services-from-hello-azure-api-management-service"></a><span data-ttu-id="fcabf-103">Pomocí externích služeb z hello služby Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fcabf-103">Using external services from hello Azure API Management service</span></span>
<span data-ttu-id="fcabf-104">Hello zásady, které jsou dostupné ve službě Azure API Management service může provádět široké škály užitečné pracovní výhradně na základě příchozího požadavku hello, hello odchozí odpovědi a informace o základní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fcabf-104">hello policies available in Azure API Management service can do a wide range of useful work based purely on hello incoming request, hello outgoing response and basic configuration information.</span></span> <span data-ttu-id="fcabf-105">Ale je možné toointeract pomocí externích služeb z rozhraní API správy zásad mnoho většího počtu možností otevře.</span><span class="sxs-lookup"><span data-stu-id="fcabf-105">However, being able toointeract with external services from API Management policies opens up many more opportunities.</span></span>

<span data-ttu-id="fcabf-106">Jsme předtím viděli, jak jsme mohou komunikovat s hello [centra událostí Azure služby pro protokolování, sledování a analýza](api-management-log-to-eventhub-sample.md).</span><span class="sxs-lookup"><span data-stu-id="fcabf-106">We have previously seen how we can interact with hello [Azure Event Hub service for logging, monitoring and analytics](api-management-log-to-eventhub-sample.md).</span></span> <span data-ttu-id="fcabf-107">V tomto článku jsme ukazují, že zásady, které vám umožňují toointeract s žádné externí HTTP na základě služby.</span><span class="sxs-lookup"><span data-stu-id="fcabf-107">In this article we will demonstrate policies that allow you toointeract with any external HTTP based service.</span></span> <span data-ttu-id="fcabf-108">Tyto zásady lze pro spuštění vzdáleného události nebo pro načtení informací, které budou použité toomanipulate hello původní žádosti a odpovědi nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="fcabf-108">These policies can be used for triggering remote events or for retrieving information that will be used toomanipulate hello original request and response in some way.</span></span>

## <a name="send-one-way-request"></a><span data-ttu-id="fcabf-109">Odeslání jeden způsob požadavků</span><span class="sxs-lookup"><span data-stu-id="fcabf-109">Send-One-Way-Request</span></span>
<span data-ttu-id="fcabf-110">Pravděpodobně hello nejjednodušší externí interakce je hello ještě efektivněji a zapomněli styl požadavek, který umožňuje externí služba toobe oznámení určitého druhu důležité události.</span><span class="sxs-lookup"><span data-stu-id="fcabf-110">Possibly hello simplest external interaction is hello fire-and-forget style of request that allows an external service toobe notified of some kind of important event.</span></span> <span data-ttu-id="fcabf-111">Můžeme použít zásady toku řízení hello `choose` toodetect jakýkoli druh podmínku, kterou jsme zajímá a poté, pokud hello podmínky splněny, abychom mohli provádět externí požadavku HTTP pomocí hello [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) zásad.</span><span class="sxs-lookup"><span data-stu-id="fcabf-111">We can use hello control flow policy `choose` toodetect any kind of condition that we are interested in and then, if hello condition is satisfied, we can make an external HTTP request using hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) policy.</span></span> <span data-ttu-id="fcabf-112">To může být žádost tooa, zasílání zpráv systému jako Hipchat nebo Slack nebo pošty rozhraní API jako sendgrid vám umožňuje nebo MailChimp, nebo pro kritické podpory incidentů něco jako PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="fcabf-112">This could be a request tooa messaging system like Hipchat or Slack, or a mail API like SendGrid or MailChimp, or for critical support incidents something like PagerDuty.</span></span> <span data-ttu-id="fcabf-113">Všechny tyto systémy zasílání zpráv mají jednoduché rozhraní API HTTP, který lze snadno vyvolat.</span><span class="sxs-lookup"><span data-stu-id="fcabf-113">All of these messaging systems have simple HTTP APIs that we can easily invoke.</span></span>

### <a name="alerting-with-slack"></a><span data-ttu-id="fcabf-114">Výstrahy s Slack</span><span class="sxs-lookup"><span data-stu-id="fcabf-114">Alerting with Slack</span></span>
<span data-ttu-id="fcabf-115">Hello následující příklad ukazuje, jak toosend tooa zpráva Slack chatovací místnosti, pokud je stavový kód odpovědi HTTP hello větší než nebo roven hodnotě too500.</span><span class="sxs-lookup"><span data-stu-id="fcabf-115">hello following example demonstrates how toosend a message tooa Slack chat room if hello HTTP response status code is greater than or equal too500.</span></span> <span data-ttu-id="fcabf-116">Rozsah 500 Chyba označuje, že k problému s naše back-end rozhraní API, která hello klienta našem rozhraní API nelze vyřešit sami.</span><span class="sxs-lookup"><span data-stu-id="fcabf-116">A 500 range error indicates a problem with our backend API that hello client of our API cannot resolve themselves.</span></span> <span data-ttu-id="fcabf-117">Obvykle vyžaduje nějaký druh zásahu na našem část.</span><span class="sxs-lookup"><span data-stu-id="fcabf-117">It usually requires some kind of intervention on our part.</span></span>  

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

<span data-ttu-id="fcabf-118">Slack má hello představu o háky příchozí webové.</span><span class="sxs-lookup"><span data-stu-id="fcabf-118">Slack has hello notion of inbound web hooks.</span></span> <span data-ttu-id="fcabf-119">Při konfiguraci příchozí webové háku, generuje Slack speciální adresy URL, která vám umožní do kanálu Slack hello toodo jednoduché požadavek POST a toopass zprávu.</span><span class="sxs-lookup"><span data-stu-id="fcabf-119">When configuring an inbound web hook, Slack generates a special URL which allows you toodo a simple POST request and toopass a message into hello Slack channel.</span></span> <span data-ttu-id="fcabf-120">text JSON, který vytváříme Hello je založena na formátu definované Slack.</span><span class="sxs-lookup"><span data-stu-id="fcabf-120">hello JSON body that we create is based on a format defined by Slack.</span></span>

![Slack webové háku](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a><span data-ttu-id="fcabf-122">Je ještě efektivněji a zapomněli dostatečně vhodné?</span><span class="sxs-lookup"><span data-stu-id="fcabf-122">Is fire and forget good enough?</span></span>
<span data-ttu-id="fcabf-123">Existují určité kompromisy při použití styl ještě efektivněji a zapomněli požadavku.</span><span class="sxs-lookup"><span data-stu-id="fcabf-123">There are certain tradeoffs when using a fire-and-forget style of request.</span></span> <span data-ttu-id="fcabf-124">Pokud z jakéhokoliv důvodu hello požadavek selže a pak nebudou oznámeny hello selhání.</span><span class="sxs-lookup"><span data-stu-id="fcabf-124">If for some reason, hello request fails, then hello failure will not be reported.</span></span> <span data-ttu-id="fcabf-125">V takovém případě konkrétní není oprávněná hello složitost systému hlášení selhání sekundárního objektu a hello další výkonová náročnost čekání na odezvu hello.</span><span class="sxs-lookup"><span data-stu-id="fcabf-125">In this particular situation, hello complexity of having a secondary failure reporting system and hello additional performance cost of waiting for hello response is not warranted.</span></span> <span data-ttu-id="fcabf-126">Pro scénáře, kdy je zásadní toocheck hello odpovědi, pak hello [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad je lepší volbou.</span><span class="sxs-lookup"><span data-stu-id="fcabf-126">For scenarios where it is essential toocheck hello response, then hello [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy is a better option.</span></span>

## <a name="send-request"></a><span data-ttu-id="fcabf-127">Odeslání žádosti</span><span class="sxs-lookup"><span data-stu-id="fcabf-127">Send-Request</span></span>
<span data-ttu-id="fcabf-128">Hello `send-request` zásad umožňuje pomocí tooperform externí služba, která je komplexní zpracování funkce a služba správy toohello rozhraní API vracená data, která lze použít pro další zpracování zásad.</span><span class="sxs-lookup"><span data-stu-id="fcabf-128">hello `send-request` policy enables using an external service tooperform complex processing functions and return data toohello API management service that can be used for further policy processing.</span></span>

### <a name="authorizing-reference-tokens"></a><span data-ttu-id="fcabf-129">Autorizace tokeny odkazů</span><span class="sxs-lookup"><span data-stu-id="fcabf-129">Authorizing reference tokens</span></span>
<span data-ttu-id="fcabf-130">Hlavní funkce API Management chrání back-endové prostředky.</span><span class="sxs-lookup"><span data-stu-id="fcabf-130">A major function of API Management is protecting backend resources.</span></span> <span data-ttu-id="fcabf-131">Pokud serveru ověřování hello používá vaše rozhraní API vytvoří [tokeny JWT](http://jwt.io/) jako součást jeho tok OAuth2, jako [Azure Active Directory](../active-directory/active-directory-aadconnect.md) nemá, můžete použít hello `validate-jwt` platnost hello tooverify zásad Hello token.</span><span class="sxs-lookup"><span data-stu-id="fcabf-131">If hello authorization server used by your API creates [JWT tokens](http://jwt.io/) as part of its OAuth2 flow, as [Azure Active Directory](../active-directory/active-directory-aadconnect.md) does, then you can use hello `validate-jwt` policy tooverify hello validity of hello token.</span></span> <span data-ttu-id="fcabf-132">Ale některé servery autorizace vytvořit, co se nazývají [odkazovat tokeny](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , nelze ověřit bez provedení serveru autorizace toohello zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="fcabf-132">However, some authorization servers create what are called [reference tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) that cannot be verified without making a call back toohello authorization server.</span></span>

### <a name="standardized-introspection"></a><span data-ttu-id="fcabf-133">Standardizovaná introspection</span><span class="sxs-lookup"><span data-stu-id="fcabf-133">Standardized introspection</span></span>
<span data-ttu-id="fcabf-134">V posledních hello byl nijak standardizované ověřit token odkazu se serverem ověřování.</span><span class="sxs-lookup"><span data-stu-id="fcabf-134">In hello past there has been no standardized way of verifying a reference token with an authorization server.</span></span> <span data-ttu-id="fcabf-135">Ale nedávno navrhovaný standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publikoval hello IETF, která definuje, jak server prostředků můžete ověřit hello platnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="fcabf-135">However a recently proposed standard [RFC 7662](https://tools.ietf.org/html/rfc7662) was published by hello IETF that defines how a resource server can verify hello validity of a token.</span></span>

### <a name="extracting-hello-token"></a><span data-ttu-id="fcabf-136">Extrahování hello tokenu</span><span class="sxs-lookup"><span data-stu-id="fcabf-136">Extracting hello token</span></span>
<span data-ttu-id="fcabf-137">prvním krokem Hello je tooextract hello tokenu z hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="fcabf-137">hello first step is tooextract hello token from hello Authorization header.</span></span> <span data-ttu-id="fcabf-138">Hodnota hlavičky Hello musí být naformátovaná hello `Bearer` autorizace schématu, jedna mezera a pak hello autorizační token dle [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="fcabf-138">hello header value should be formatted with hello `Bearer` authorization scheme, a single space and then hello authorization token as per [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span></span> <span data-ttu-id="fcabf-139">Bohužel existují případech, kdy je vynechán hello autorizace schéma.</span><span class="sxs-lookup"><span data-stu-id="fcabf-139">Unfortunately there are cases where hello authorization scheme is omitted.</span></span> <span data-ttu-id="fcabf-140">tooaccount to při analýze, jsme rozdělit hello hodnota záhlaví na místo a vyberte hello poslední řetězec z hello vrátila pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="fcabf-140">tooaccount for this when parsing, we split hello header value on a space and select hello last string from hello returned array of strings.</span></span> <span data-ttu-id="fcabf-141">To poskytuje řešení chybně formátovaný autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="fcabf-141">This provides a workaround for badly formatted authorization headers.</span></span>

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a><span data-ttu-id="fcabf-142">Vytváření hello ověření požadavku</span><span class="sxs-lookup"><span data-stu-id="fcabf-142">Making hello validation request</span></span>
<span data-ttu-id="fcabf-143">Jakmile jsme hello autorizační token, abychom mohli provádět hello požadavek toovalidate hello token.</span><span class="sxs-lookup"><span data-stu-id="fcabf-143">Once we have hello authorization token, we can make hello request toovalidate hello token.</span></span> <span data-ttu-id="fcabf-144">RFC 7662 volá tento proces introspection a vyžaduje, aby vám `POST` prostředku introspection toohello formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="fcabf-144">RFC 7662 calls this process introspection and requires that you `POST` a HTML form toohello introspection resource.</span></span> <span data-ttu-id="fcabf-145">Hello formuláře HTML musí obsahovat alespoň dvojici klíč/hodnota s klíčem hello `token`.</span><span class="sxs-lookup"><span data-stu-id="fcabf-145">hello HTML form must at least contain a key/value pair with hello key `token`.</span></span> <span data-ttu-id="fcabf-146">Tento požadavek toohello autorizace server musí být také ověřené tooensure, který nelze přejít vlečným škodlivý klientů pro platné tokeny.</span><span class="sxs-lookup"><span data-stu-id="fcabf-146">This request toohello authorization server must also be authenticated tooensure that malicious clients cannot go trawling for valid tokens.</span></span>

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

### <a name="checking-hello-response"></a><span data-ttu-id="fcabf-147">Kontrola, zda text hello odpovědi</span><span class="sxs-lookup"><span data-stu-id="fcabf-147">Checking hello response</span></span>
<span data-ttu-id="fcabf-148">Hello `response-variable-name` atribut je použité toogive přístup hello vrátil odpověď.</span><span class="sxs-lookup"><span data-stu-id="fcabf-148">hello `response-variable-name` attribute is used toogive access hello returned response.</span></span> <span data-ttu-id="fcabf-149">Hello název definovaný v této vlastnosti lze použít jako klíč do hello `context.Variables` slovník tooaccess hello `IResponse` objektu.</span><span class="sxs-lookup"><span data-stu-id="fcabf-149">hello name defined in this property can be used as a key into hello `context.Variables` dictionary tooaccess hello `IResponse` object.</span></span>

<span data-ttu-id="fcabf-150">Z objektu odpovědi hello načteme textu hello a RFC 7622 víme, že hello odpovědi musí být objekt JSON a musí obsahovat aspoň vlastnost s názvem `active` tedy logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fcabf-150">From hello response object we can retrieve hello body and RFC 7622 tells us that hello response must be a JSON object and must contain at least a property called `active` that is a boolean value.</span></span> <span data-ttu-id="fcabf-151">Když `active` má hodnotu true, pak hello token je považován za platný.</span><span class="sxs-lookup"><span data-stu-id="fcabf-151">When `active` is true then hello token is considered valid.</span></span>

### <a name="reporting-failure"></a><span data-ttu-id="fcabf-152">Generování sestav selhání</span><span class="sxs-lookup"><span data-stu-id="fcabf-152">Reporting failure</span></span>
<span data-ttu-id="fcabf-153">Používáme `<choose>` zásad toodetect Pokud hello token je neplatný a pokud ano, vrátí odpovědi 401.</span><span class="sxs-lookup"><span data-stu-id="fcabf-153">We use a `<choose>` policy toodetect if hello token is invalid and if so, return a 401 response.</span></span>

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

<span data-ttu-id="fcabf-154">Dle [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) který popisuje, jak `bearer` slouží tokeny, také vrátíme `WWW-Authenticate` hlavičky odpovědi hello 401.</span><span class="sxs-lookup"><span data-stu-id="fcabf-154">As per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) which describes how `bearer` tokens should be used, we also return a `WWW-Authenticate` header with hello 401 response.</span></span> <span data-ttu-id="fcabf-155">Hello WWW-Authenticate je určený tooinstruct klienta o tooconstruct žádost správně autorizované.</span><span class="sxs-lookup"><span data-stu-id="fcabf-155">hello WWW-Authenticate is intended tooinstruct a client on how tooconstruct a properly authorized request.</span></span> <span data-ttu-id="fcabf-156">Z důvodu toohello širokou škálu možných s hello OAuth2 framework přístupy, je obtížné toocommunicate hello všechny potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="fcabf-156">Due toohello wide variety of approaches possible with hello OAuth2 framework, it is difficult toocommunicate all hello needed information.</span></span> <span data-ttu-id="fcabf-157">Naštěstí existuje úsilí zpracováváme toohelp [klienti zjišťovat, jak tooproperly autorizaci požadavků tooa prostředků serveru](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span><span class="sxs-lookup"><span data-stu-id="fcabf-157">Fortunately there are efforts underway toohelp [clients discover how tooproperly authorize requests tooa resource server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span></span>

### <a name="final-solution"></a><span data-ttu-id="fcabf-158">Konečné řešení</span><span class="sxs-lookup"><span data-stu-id="fcabf-158">Final solution</span></span>
<span data-ttu-id="fcabf-159">Všechny součásti hello uvedení společně, se nám získat hello následující zásady:</span><span class="sxs-lookup"><span data-stu-id="fcabf-159">Putting all hello pieces together, we get hello following policy:</span></span>

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

<span data-ttu-id="fcabf-160">Toto je pouze jeden z mnoha příklady, jak hello `send-request` zásad může být užitečné externích služeb používaných toointegrate do procesu hello požadavků a odpovědí prostřednictvím hello služba API Management.</span><span class="sxs-lookup"><span data-stu-id="fcabf-160">This is only one of many examples of how hello `send-request` policy can be used toointegrate useful external services into hello process of requests and responses flowing through hello API Management service.</span></span>

## <a name="response-composition"></a><span data-ttu-id="fcabf-161">Složení odpovědi</span><span class="sxs-lookup"><span data-stu-id="fcabf-161">Response Composition</span></span>
<span data-ttu-id="fcabf-162">Hello `send-request` zásadu lze použít pro rozšíření systému primární požadavek tooa back-end, jako jsme viděli v předchozím příkladu hello, nebo můžete využít jako kompletní nahrazení pro volání hello back-end.</span><span class="sxs-lookup"><span data-stu-id="fcabf-162">hello `send-request` policy can be used for enhancing a primary request tooa backend system, as we saw in hello previous example, or it can be used as a complete replace for of hello backend call.</span></span> <span data-ttu-id="fcabf-163">Touto technikou jsme můžete snadno vytvořit složené prostředky, které jsou seskupeny z několika různými systémy.</span><span class="sxs-lookup"><span data-stu-id="fcabf-163">Using this technique we can easily create composite resources that are aggregated from multiple different systems.</span></span>

### <a name="building-a-dashboard"></a><span data-ttu-id="fcabf-164">Vytváření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="fcabf-164">Building a dashboard</span></span>
<span data-ttu-id="fcabf-165">Někdy budete chtít toobe možné tooexpose informace, které existuje ve více back-end systémy, například toodrive řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="fcabf-165">Sometimes you want toobe able tooexpose information that exists in multiple backend systems, for example, toodrive a dashboard.</span></span> <span data-ttu-id="fcabf-166">Hello klíčových ukazatelů výkonu pocházet ze všech různých back EndY, ale si přejete není toothem tooprovide přímý přístup a bude dobrý, v jedné žádosti může načíst všechny informace o hello.</span><span class="sxs-lookup"><span data-stu-id="fcabf-166">hello KPIs come from all different back-ends, but you would prefer not tooprovide direct access toothem and it would be nice if all hello information could be retrieved in a single request.</span></span> <span data-ttu-id="fcabf-167">Možná některé z informací hello back-end vyžaduje některé segmentování a analyzování a úpravě trochu nejprve!</span><span class="sxs-lookup"><span data-stu-id="fcabf-167">Perhaps some of hello backend information needs some slicing and dicing and a little sanitizing first!</span></span> <span data-ttu-id="fcabf-168">Je možné toocache že složené prostředků budou užitečné tooreduce back-end hello načíst jako víte, že uživatelé mají která podporují ražením klávesu F5 hello v toosee pořadí, pokud jejich underperforming metriky se může změnit.</span><span class="sxs-lookup"><span data-stu-id="fcabf-168">Being able toocache that composite resource would be a useful tooreduce hello backend load as you know users have a habit of hammering hello F5 key in order toosee if their underperforming metrics might change.</span></span>    

### <a name="faking-hello-resource"></a><span data-ttu-id="fcabf-169">Faking hello prostředků</span><span class="sxs-lookup"><span data-stu-id="fcabf-169">Faking hello resource</span></span>
<span data-ttu-id="fcabf-170">první krok toobuilding Hello naše prostředek řídicí panel je tooconfigure operaci nového portálu vydavatele hello API Management.</span><span class="sxs-lookup"><span data-stu-id="fcabf-170">hello first step toobuilding our dashboard resource is tooconfigure a new operation in hello API Management publisher portal.</span></span> <span data-ttu-id="fcabf-171">Toto bude naše zásady toobuild složení tooconfigure operace použité zástupný symbol naše dynamické prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcabf-171">This will be a placeholder operation used tooconfigure our composition policy toobuild our dynamic resource.</span></span>

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a><span data-ttu-id="fcabf-173">Provádění požadavků hello</span><span class="sxs-lookup"><span data-stu-id="fcabf-173">Making hello requests</span></span>
<span data-ttu-id="fcabf-174">Jednou hello `dashboard` operace byla vytvořena jsme můžete nakonfigurovat zásadu speciálně pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="fcabf-174">Once hello `dashboard` operation has been created we can configure a policy specifically for that operation.</span></span> 

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

<span data-ttu-id="fcabf-176">prvním krokem Hello je tooextract žádné parametry dotazu z příchozího požadavku hello, takže jsme může předávat tooour back-end.</span><span class="sxs-lookup"><span data-stu-id="fcabf-176">hello first step  is tooextract any query parameters from hello incoming request, so that we can forward them tooour backend.</span></span> <span data-ttu-id="fcabf-177">V tomto příkladu se naše řídicí panel zobrazuje informace založené na určitou dobu tedy `fromDate` a `toDate` parametr.</span><span class="sxs-lookup"><span data-stu-id="fcabf-177">In this example our dashboard is showing information based on a period of time an therefore has a `fromDate` and `toDate` parameter.</span></span> <span data-ttu-id="fcabf-178">Můžeme použít hello `set-variable` informace o zásadách tooextract hello z adresy URL žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="fcabf-178">We can use hello `set-variable` policy tooextract hello information from hello request URL.</span></span>

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

<span data-ttu-id="fcabf-179">Jakmile tyto informace můžeme provádět požadavky tooall hello back-end systémy.</span><span class="sxs-lookup"><span data-stu-id="fcabf-179">Once we have this information we can make requests tooall hello backend systems.</span></span> <span data-ttu-id="fcabf-180">Každý požadavek vytvoří novou adresu URL s informace o parametrech hello volá jeho příslušný server a ukládá hello odpovědi v kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="fcabf-180">Each request constructs a new URL with hello parameter information and calls its respective server and stores hello response in a context variable.</span></span>

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

<span data-ttu-id="fcabf-181">Tyto požadavky bude spuštěn v pořadí, což není ideální.</span><span class="sxs-lookup"><span data-stu-id="fcabf-181">These requests will execute in sequence, which is not ideal.</span></span> <span data-ttu-id="fcabf-182">V budoucích vydání jsme zavádí nové zásady názvem `wait` , povolí všechny tyto požadavky tooexecute paralelně.</span><span class="sxs-lookup"><span data-stu-id="fcabf-182">In an upcoming release we will be introducing a new policy called `wait` that will enable all of these requests tooexecute in parallel.</span></span>

### <a name="responding"></a><span data-ttu-id="fcabf-183">Neodpovídá</span><span class="sxs-lookup"><span data-stu-id="fcabf-183">Responding</span></span>
<span data-ttu-id="fcabf-184">tooconstruct hello složené odpovědi používáme hello [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásad.</span><span class="sxs-lookup"><span data-stu-id="fcabf-184">tooconstruct hello composite response we can use hello [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policy.</span></span> <span data-ttu-id="fcabf-185">Hello `set-body` element můžete použít výrazu tooconstruct nový `JObject` s všechny součásti reprezentace hello vložených jako vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fcabf-185">hello `set-body` element can use an expression tooconstruct a new `JObject` with all hello component representations embedded as properties.</span></span>

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

<span data-ttu-id="fcabf-186">dokončení zásad Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="fcabf-186">hello complete policy looks as follows:</span></span>

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

<span data-ttu-id="fcabf-187">V konfiguraci hello hello zástupný symbol operace, které jsme můžete konfigurovat hello řídicí panel prostředků toobe uložená v mezipaměti pro alespoň jednu hodinu vzhledem k tomu, že jsme srozumitelná hello povaha hello dat znamená, že i když je zastaralý, že budou mít pořád dostatečně efektivní hodinu Uživatelé toohello tooconvey cenné informace.</span><span class="sxs-lookup"><span data-stu-id="fcabf-187">In hello configuration of hello placeholder operation we can configure hello dashboard resource toobe cached for at least an hour because we understand hello nature of hello data means that even if it is an hour out of date, it will still be sufficiently effective tooconvey valuable information toohello users.</span></span>

## <a name="summary"></a><span data-ttu-id="fcabf-188">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fcabf-188">Summary</span></span>
<span data-ttu-id="fcabf-189">Služba Azure API Management nabízí flexibilní zásady, které je možné selektivně použít tooHTTP provoz a umožňuje složení služeb back-end.</span><span class="sxs-lookup"><span data-stu-id="fcabf-189">Azure API Management service provides flexible policies that can be selectively applied tooHTTP traffic and enables composition of backend services.</span></span> <span data-ttu-id="fcabf-190">Jestli chcete tooenhance bránu rozhraní API s výstrahy funkce, ověření, možnosti ověření nebo vytvořit nové složené prostředky v závislosti na více služeb back-end, hello `send-request` a související zásady otevřete široké možnosti.</span><span class="sxs-lookup"><span data-stu-id="fcabf-190">Whether you want tooenhance your API gateway with alerting functions, verification, validation capabilities or create new composite resources based on multiple backend services, hello `send-request` and related policies open a world of possibilities.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="fcabf-191">Podívejte se na video s přehledem těchto zásad</span><span class="sxs-lookup"><span data-stu-id="fcabf-191">Watch a video overview of these policies</span></span>
<span data-ttu-id="fcabf-192">Další informace o hello [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), a [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásady popsaná v tomto článku, podívejte se prosím hello následující video.</span><span class="sxs-lookup"><span data-stu-id="fcabf-192">For more information on hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), and [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

