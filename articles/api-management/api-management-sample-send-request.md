---
title: "Použití služby API Management pro generování požadavků HTTP"
description: "Naučit se používat zásady žádostí a odpovědí ve službě API Management volat externí služby z rozhraní API"
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
ms.openlocfilehash: e778943715d6ca5256ad612d82bdc1f82197df0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-external-services-from-the-azure-api-management-service"></a><span data-ttu-id="6b822-103">Pomocí externích služeb ze služby Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6b822-103">Using external services from the Azure API Management service</span></span>
<span data-ttu-id="6b822-104">Zásady, které jsou dostupné ve službě Azure API Management service může provádět široké škály užitečné pracovní výhradně na základě příchozího požadavku, odchozí odpovědi a informace o základní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6b822-104">The policies available in Azure API Management service can do a wide range of useful work based purely on the incoming request, the outgoing response and basic configuration information.</span></span> <span data-ttu-id="6b822-105">Ale schopnost komunikovat s externích služeb z rozhraní API správy zásad mnoho většího počtu možností otevře.</span><span class="sxs-lookup"><span data-stu-id="6b822-105">However, being able to interact with external services from API Management policies opens up many more opportunities.</span></span>

<span data-ttu-id="6b822-106">Jsme předtím viděli, jak jsme mohou komunikovat s [centra událostí Azure služby pro protokolování, sledování a analýza](api-management-log-to-eventhub-sample.md).</span><span class="sxs-lookup"><span data-stu-id="6b822-106">We have previously seen how we can interact with the [Azure Event Hub service for logging, monitoring and analytics](api-management-log-to-eventhub-sample.md).</span></span> <span data-ttu-id="6b822-107">V tomto článku jsme ukazují, že zásady, které vám umožní komunikovat s žádné externí HTTP na základě služby.</span><span class="sxs-lookup"><span data-stu-id="6b822-107">In this article we will demonstrate policies that allow you to interact with any external HTTP based service.</span></span> <span data-ttu-id="6b822-108">Tyto zásady lze pro spuštění vzdáleného události nebo pro načtení informací, který se použije k manipulaci s původní žádosti a odpovědi nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="6b822-108">These policies can be used for triggering remote events or for retrieving information that will be used to manipulate the original request and response in some way.</span></span>

## <a name="send-one-way-request"></a><span data-ttu-id="6b822-109">Odeslání jeden způsob požadavků</span><span class="sxs-lookup"><span data-stu-id="6b822-109">Send-One-Way-Request</span></span>
<span data-ttu-id="6b822-110">Může být nejjednodušší externí interakce je ještě efektivněji a zapomněli styl požadavek, který umožňuje externí služba mají být informování určitého druhu důležité události.</span><span class="sxs-lookup"><span data-stu-id="6b822-110">Possibly the simplest external interaction is the fire-and-forget style of request that allows an external service to be notified of some kind of important event.</span></span> <span data-ttu-id="6b822-111">Můžeme použít zásady řízení toku `choose` ke zjištění jakékoliv podmínky, že jsme mají zájem o a poté, pokud je splněna podmínka, abychom mohli provádět k externí HTTP žádosti pomocí [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) zásad.</span><span class="sxs-lookup"><span data-stu-id="6b822-111">We can use the control flow policy `choose` to detect any kind of condition that we are interested in and then, if the condition is satisfied, we can make an external HTTP request using the [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) policy.</span></span> <span data-ttu-id="6b822-112">To může být žádost o zasílání zpráv systému jako Hipchat nebo Slack nebo pošty rozhraní API jako sendgrid vám umožňuje nebo MailChimp, nebo pro kritické podpory incidentů něco jako PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="6b822-112">This could be a request to a messaging system like Hipchat or Slack, or a mail API like SendGrid or MailChimp, or for critical support incidents something like PagerDuty.</span></span> <span data-ttu-id="6b822-113">Všechny tyto systémy zasílání zpráv mají jednoduché rozhraní API HTTP, který lze snadno vyvolat.</span><span class="sxs-lookup"><span data-stu-id="6b822-113">All of these messaging systems have simple HTTP APIs that we can easily invoke.</span></span>

### <a name="alerting-with-slack"></a><span data-ttu-id="6b822-114">Výstrahy s Slack</span><span class="sxs-lookup"><span data-stu-id="6b822-114">Alerting with Slack</span></span>
<span data-ttu-id="6b822-115">Následující příklad ukazuje, jak odeslat zprávu do Slack chatovací místnosti, pokud stavový kód odpovědi HTTP je větší než nebo rovna hodnotě 500.</span><span class="sxs-lookup"><span data-stu-id="6b822-115">The following example demonstrates how to send a message to a Slack chat room if the HTTP response status code is greater than or equal to 500.</span></span> <span data-ttu-id="6b822-116">Rozsah 500 Chyba znamená problém s naše back-end rozhraní API, klient našem rozhraní API nelze vyřešit sami.</span><span class="sxs-lookup"><span data-stu-id="6b822-116">A 500 range error indicates a problem with our backend API that the client of our API cannot resolve themselves.</span></span> <span data-ttu-id="6b822-117">Obvykle vyžaduje nějaký druh zásahu na našem část.</span><span class="sxs-lookup"><span data-stu-id="6b822-117">It usually requires some kind of intervention on our part.</span></span>  

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

<span data-ttu-id="6b822-118">Slack má představu o háky příchozí webové.</span><span class="sxs-lookup"><span data-stu-id="6b822-118">Slack has the notion of inbound web hooks.</span></span> <span data-ttu-id="6b822-119">Při konfiguraci příchozí webové háku, generuje Slack speciální adresy URL, která umožňuje provést jednoduchý požadavek POST a předávat zprávy do kanálu Slack.</span><span class="sxs-lookup"><span data-stu-id="6b822-119">When configuring an inbound web hook, Slack generates a special URL which allows you to do a simple POST request and to pass a message into the Slack channel.</span></span> <span data-ttu-id="6b822-120">Text JSON, který vytváříme je založena na formátu definované Slack.</span><span class="sxs-lookup"><span data-stu-id="6b822-120">The JSON body that we create is based on a format defined by Slack.</span></span>

![Slack webové háku](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a><span data-ttu-id="6b822-122">Je ještě efektivněji a zapomněli dostatečně vhodné?</span><span class="sxs-lookup"><span data-stu-id="6b822-122">Is fire and forget good enough?</span></span>
<span data-ttu-id="6b822-123">Existují určité kompromisy při použití styl ještě efektivněji a zapomněli požadavku.</span><span class="sxs-lookup"><span data-stu-id="6b822-123">There are certain tradeoffs when using a fire-and-forget style of request.</span></span> <span data-ttu-id="6b822-124">Pokud z nějakého důvodu, žádost skončí s chybou a potom selhání, nebudou ohlášena.</span><span class="sxs-lookup"><span data-stu-id="6b822-124">If for some reason, the request fails, then the failure will not be reported.</span></span> <span data-ttu-id="6b822-125">V takovém případě konkrétní není oprávněná složitost toho, že sekundární selhání sestav systému a další dopady čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="6b822-125">In this particular situation, the complexity of having a secondary failure reporting system and the additional performance cost of waiting for the response is not warranted.</span></span> <span data-ttu-id="6b822-126">Pro scénáře, kdy je nezbytně nutné, aby Zkontrolujte odpověď, a pak se [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad je lepší volbou.</span><span class="sxs-lookup"><span data-stu-id="6b822-126">For scenarios where it is essential to check the response, then the [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy is a better option.</span></span>

## <a name="send-request"></a><span data-ttu-id="6b822-127">Odeslání žádosti</span><span class="sxs-lookup"><span data-stu-id="6b822-127">Send-Request</span></span>
<span data-ttu-id="6b822-128">`send-request` Zásad umožňuje pomocí externí služby k provedení funkce komplexní zpracování a vrátit data správy rozhraní API služby, který lze použít pro další zpracování zásad.</span><span class="sxs-lookup"><span data-stu-id="6b822-128">The `send-request` policy enables using an external service to perform complex processing functions and return data to the API management service that can be used for further policy processing.</span></span>

### <a name="authorizing-reference-tokens"></a><span data-ttu-id="6b822-129">Autorizace tokeny odkazů</span><span class="sxs-lookup"><span data-stu-id="6b822-129">Authorizing reference tokens</span></span>
<span data-ttu-id="6b822-130">Hlavní funkce API Management chrání back-endové prostředky.</span><span class="sxs-lookup"><span data-stu-id="6b822-130">A major function of API Management is protecting backend resources.</span></span> <span data-ttu-id="6b822-131">Pokud server ověřování používá vaše rozhraní API vytvoří [tokeny JWT](http://jwt.io/) jako součást jeho tok OAuth2, jako [Azure Active Directory](../active-directory/active-directory-aadconnect.md) nemá, můžete použít `validate-jwt` zásad k ověření platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="6b822-131">If the authorization server used by your API creates [JWT tokens](http://jwt.io/) as part of its OAuth2 flow, as [Azure Active Directory](../active-directory/active-directory-aadconnect.md) does, then you can use the `validate-jwt` policy to verify the validity of the token.</span></span> <span data-ttu-id="6b822-132">Ale některé servery autorizace vytvořit, co se nazývají [odkazovat tokeny](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , nelze ověřit bez provedení volání zpět na server ověřování.</span><span class="sxs-lookup"><span data-stu-id="6b822-132">However, some authorization servers create what are called [reference tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) that cannot be verified without making a call back to the authorization server.</span></span>

### <a name="standardized-introspection"></a><span data-ttu-id="6b822-133">Standardizovaná introspection</span><span class="sxs-lookup"><span data-stu-id="6b822-133">Standardized introspection</span></span>
<span data-ttu-id="6b822-134">V minulosti byl nijak standardizované ověřit token odkazu se serverem ověřování.</span><span class="sxs-lookup"><span data-stu-id="6b822-134">In the past there has been no standardized way of verifying a reference token with an authorization server.</span></span> <span data-ttu-id="6b822-135">Ale nedávno navrhovaný standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publikoval IETF, která definuje, jak server prostředků můžete ověřit platnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="6b822-135">However a recently proposed standard [RFC 7662](https://tools.ietf.org/html/rfc7662) was published by the IETF that defines how a resource server can verify the validity of a token.</span></span>

### <a name="extracting-the-token"></a><span data-ttu-id="6b822-136">Extrahování tokenu</span><span class="sxs-lookup"><span data-stu-id="6b822-136">Extracting the token</span></span>
<span data-ttu-id="6b822-137">Prvním krokem je extrahuje token z autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="6b822-137">The first step is to extract the token from the Authorization header.</span></span> <span data-ttu-id="6b822-138">Hodnota hlavičky by měl být naformátován pomocí `Bearer` autorizace schéma a jedna mezera a autorizaci token dle [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="6b822-138">The header value should be formatted with the `Bearer` authorization scheme, a single space and then the authorization token as per [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span></span> <span data-ttu-id="6b822-139">Bohužel existují případech, kdy je vynechán schéma autorizace.</span><span class="sxs-lookup"><span data-stu-id="6b822-139">Unfortunately there are cases where the authorization scheme is omitted.</span></span> <span data-ttu-id="6b822-140">Chcete-li pro tento účet při analýze, jsme rozdělit hodnota hlavičky v prostoru a vyberte možnost poslední řetězec vrácený pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="6b822-140">To account for this when parsing, we split the header value on a space and select the last string from the returned array of strings.</span></span> <span data-ttu-id="6b822-141">To poskytuje řešení chybně formátovaný autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="6b822-141">This provides a workaround for badly formatted authorization headers.</span></span>

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a><span data-ttu-id="6b822-142">Vytvoření ověření požadavku</span><span class="sxs-lookup"><span data-stu-id="6b822-142">Making the validation request</span></span>
<span data-ttu-id="6b822-143">Jakmile jsme autorizačním tokenem, abychom mohli provádět ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="6b822-143">Once we have the authorization token, we can make the request to validate the token.</span></span> <span data-ttu-id="6b822-144">RFC 7662 volá tento proces introspection a vyžaduje, aby vám `POST` formuláře HTML k introspection prostředku.</span><span class="sxs-lookup"><span data-stu-id="6b822-144">RFC 7662 calls this process introspection and requires that you `POST` a HTML form to the introspection resource.</span></span> <span data-ttu-id="6b822-145">Formuláře HTML musí obsahovat alespoň dvojici klíč/hodnota s klíčem `token`.</span><span class="sxs-lookup"><span data-stu-id="6b822-145">The HTML form must at least contain a key/value pair with the key `token`.</span></span> <span data-ttu-id="6b822-146">Tento požadavek server ověřování musí být ověřeny také zajistit, že škodlivý klientů nelze přejít vlečným pro platné tokeny.</span><span class="sxs-lookup"><span data-stu-id="6b822-146">This request to the authorization server must also be authenticated to ensure that malicious clients cannot go trawling for valid tokens.</span></span>

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

### <a name="checking-the-response"></a><span data-ttu-id="6b822-147">Kontrola odpovědi</span><span class="sxs-lookup"><span data-stu-id="6b822-147">Checking the response</span></span>
<span data-ttu-id="6b822-148">`response-variable-name` Atribut se používá k poskytnout přístup vrácený odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6b822-148">The `response-variable-name` attribute is used to give access the returned response.</span></span> <span data-ttu-id="6b822-149">Název definovaný v této vlastnosti lze použít jako klíč do `context.Variables` slovník pro přístup `IResponse` objektu.</span><span class="sxs-lookup"><span data-stu-id="6b822-149">The name defined in this property can be used as a key into the `context.Variables` dictionary to access the `IResponse` object.</span></span>

<span data-ttu-id="6b822-150">Z objektu odpovědi můžeme načíst tělo a RFC 7622 víme, že odpověď musí být objekt JSON a musí obsahovat aspoň vlastnost s názvem `active` tedy logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6b822-150">From the response object we can retrieve the body and RFC 7622 tells us that the response must be a JSON object and must contain at least a property called `active` that is a boolean value.</span></span> <span data-ttu-id="6b822-151">Když `active` má hodnotu true, pak token je považován za platný.</span><span class="sxs-lookup"><span data-stu-id="6b822-151">When `active` is true then the token is considered valid.</span></span>

### <a name="reporting-failure"></a><span data-ttu-id="6b822-152">Generování sestav selhání</span><span class="sxs-lookup"><span data-stu-id="6b822-152">Reporting failure</span></span>
<span data-ttu-id="6b822-153">Používáme `<choose>` zásady a zjistit, jestli je neplatný token a pokud ano, vrátí odpovědi 401.</span><span class="sxs-lookup"><span data-stu-id="6b822-153">We use a `<choose>` policy to detect if the token is invalid and if so, return a 401 response.</span></span>

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

<span data-ttu-id="6b822-154">Dle [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) který popisuje, jak `bearer` slouží tokeny, také vrátíme `WWW-Authenticate` hlavička s odpovědi 401.</span><span class="sxs-lookup"><span data-stu-id="6b822-154">As per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) which describes how `bearer` tokens should be used, we also return a `WWW-Authenticate` header with the 401 response.</span></span> <span data-ttu-id="6b822-155">WWW-Authenticate je určený dáte pokyn, aby klient o tom, jak vytvořit žádost o správně autorizované.</span><span class="sxs-lookup"><span data-stu-id="6b822-155">The WWW-Authenticate is intended to instruct a client on how to construct a properly authorized request.</span></span> <span data-ttu-id="6b822-156">Z důvodu širokou škálu přístupy možné pomocí rozhraní OAuth2 je obtížné komunikaci všechny potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="6b822-156">Due to the wide variety of approaches possible with the OAuth2 framework, it is difficult to communicate all the needed information.</span></span> <span data-ttu-id="6b822-157">Naštěstí existují úsilí zpracováváme na pomoc [klienti zjišťovat správně autorizace požadavky na server prostředků](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span><span class="sxs-lookup"><span data-stu-id="6b822-157">Fortunately there are efforts underway to help [clients discover how to properly authorize requests to a resource server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span></span>

### <a name="final-solution"></a><span data-ttu-id="6b822-158">Konečné řešení</span><span class="sxs-lookup"><span data-stu-id="6b822-158">Final solution</span></span>
<span data-ttu-id="6b822-159">Spojení všech částí společně, se nám získat tyto zásady:</span><span class="sxs-lookup"><span data-stu-id="6b822-159">Putting all the pieces together, we get the following policy:</span></span>

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
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

<span data-ttu-id="6b822-160">Toto je pouze jeden z mnoha příklady jak `send-request` zásadu lze použít k integraci užitečné externích služeb do procesu požadavků a odpovědí prostřednictvím služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6b822-160">This is only one of many examples of how the `send-request` policy can be used to integrate useful external services into the process of requests and responses flowing through the API Management service.</span></span>

## <a name="response-composition"></a><span data-ttu-id="6b822-161">Složení odpovědi</span><span class="sxs-lookup"><span data-stu-id="6b822-161">Response Composition</span></span>
<span data-ttu-id="6b822-162">`send-request` Zásadu lze použít pro zlepšení primární požadavek na back-end systému, jako jsme viděli v předchozím příkladu, nebo můžete využít jako kompletní nahrazení pro volání back-end.</span><span class="sxs-lookup"><span data-stu-id="6b822-162">The `send-request` policy can be used for enhancing a primary request to a backend system, as we saw in the previous example, or it can be used as a complete replace for of the backend call.</span></span> <span data-ttu-id="6b822-163">Touto technikou jsme můžete snadno vytvořit složené prostředky, které jsou seskupeny z několika různými systémy.</span><span class="sxs-lookup"><span data-stu-id="6b822-163">Using this technique we can easily create composite resources that are aggregated from multiple different systems.</span></span>

### <a name="building-a-dashboard"></a><span data-ttu-id="6b822-164">Vytváření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="6b822-164">Building a dashboard</span></span>
<span data-ttu-id="6b822-165">Někdy budete chtít mít ke zveřejnění informací, že existuje více back-end systémy, třeba, aby jednotka řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="6b822-165">Sometimes you want to be able to expose information that exists in multiple backend systems, for example, to drive a dashboard.</span></span> <span data-ttu-id="6b822-166">Klíčové ukazatele výkonu pocházet z všechny různé back EndY, ale chcete raději nechcete poskytuje přímý přístup k nim a bude dobrý, všechny informace, které se nepovedlo načíst v jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="6b822-166">The KPIs come from all different back-ends, but you would prefer not to provide direct access to them and it would be nice if all the information could be retrieved in a single request.</span></span> <span data-ttu-id="6b822-167">Možná některé z informací back-end vyžaduje některé segmentování a analyzování a úpravě trochu nejprve!</span><span class="sxs-lookup"><span data-stu-id="6b822-167">Perhaps some of the backend information needs some slicing and dicing and a little sanitizing first!</span></span> <span data-ttu-id="6b822-168">Schopnost mezipaměti složené prostředku by být užitečné ke snížení zatížení back-end, jako je zřejmé, že uživatelé mají která podporují ražením klávesu F5, aby bylo možné zjistit, zda se může změnit jejich underperforming metriky.</span><span class="sxs-lookup"><span data-stu-id="6b822-168">Being able to cache that composite resource would be a useful to reduce the backend load as you know users have a habit of hammering the F5 key in order to see if their underperforming metrics might change.</span></span>    

### <a name="faking-the-resource"></a><span data-ttu-id="6b822-169">Faking prostředku</span><span class="sxs-lookup"><span data-stu-id="6b822-169">Faking the resource</span></span>
<span data-ttu-id="6b822-170">Prvním krokem k vytváření prostředků naše řídicí panel je konfigurace nového operace v portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6b822-170">The first step to building our dashboard resource is to configure a new operation in the API Management publisher portal.</span></span> <span data-ttu-id="6b822-171">Bude jím operaci na zástupný symbol slouží ke konfiguraci zásad naše složení k sestavení naše dynamické prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b822-171">This will be a placeholder operation used to configure our composition policy to build our dynamic resource.</span></span>

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a><span data-ttu-id="6b822-173">Provedení žádosti</span><span class="sxs-lookup"><span data-stu-id="6b822-173">Making the requests</span></span>
<span data-ttu-id="6b822-174">Jednou `dashboard` operace byla vytvořena jsme můžete nakonfigurovat zásadu speciálně pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="6b822-174">Once the `dashboard` operation has been created we can configure a policy specifically for that operation.</span></span> 

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

<span data-ttu-id="6b822-176">Prvním krokem je extrahovat žádné parametry dotazu z příchozího požadavku, takže jsme může předávat na našem back-end.</span><span class="sxs-lookup"><span data-stu-id="6b822-176">The first step  is to extract any query parameters from the incoming request, so that we can forward them to our backend.</span></span> <span data-ttu-id="6b822-177">V tomto příkladu se naše řídicí panel zobrazuje informace založené na určitou dobu tedy `fromDate` a `toDate` parametr.</span><span class="sxs-lookup"><span data-stu-id="6b822-177">In this example our dashboard is showing information based on a period of time an therefore has a `fromDate` and `toDate` parameter.</span></span> <span data-ttu-id="6b822-178">Můžeme použít `set-variable` zásad k extrahování informací z adresy URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="6b822-178">We can use the `set-variable` policy to extract the information from the request URL.</span></span>

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

<span data-ttu-id="6b822-179">Jakmile tyto informace bychom mohli požadavky na back-end systémy.</span><span class="sxs-lookup"><span data-stu-id="6b822-179">Once we have this information we can make requests to all the backend systems.</span></span> <span data-ttu-id="6b822-180">Každý požadavek vytvoří novou adresu URL s informace o parametru a volá jeho příslušný server a ukládá odpovědi v kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="6b822-180">Each request constructs a new URL with the parameter information and calls its respective server and stores the response in a context variable.</span></span>

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

<span data-ttu-id="6b822-181">Tyto požadavky bude spuštěn v pořadí, což není ideální.</span><span class="sxs-lookup"><span data-stu-id="6b822-181">These requests will execute in sequence, which is not ideal.</span></span> <span data-ttu-id="6b822-182">V budoucích vydání jsme zavádí nové zásady názvem `wait` , povolí všechny tyto požadavky na spouštění paralelně.</span><span class="sxs-lookup"><span data-stu-id="6b822-182">In an upcoming release we will be introducing a new policy called `wait` that will enable all of these requests to execute in parallel.</span></span>

### <a name="responding"></a><span data-ttu-id="6b822-183">Neodpovídá</span><span class="sxs-lookup"><span data-stu-id="6b822-183">Responding</span></span>
<span data-ttu-id="6b822-184">K vytvoření složeného odpovědi můžeme použít [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásad.</span><span class="sxs-lookup"><span data-stu-id="6b822-184">To construct the composite response we can use the [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policy.</span></span> <span data-ttu-id="6b822-185">`set-body` Element výraz můžete použít k vytvoření nové `JObject` s všechny součásti reprezentace vložených jako vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6b822-185">The `set-body` element can use an expression to construct a new `JObject` with all the component representations embedded as properties.</span></span>

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

<span data-ttu-id="6b822-186">Dokončení zásad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6b822-186">The complete policy looks as follows:</span></span>

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

<span data-ttu-id="6b822-187">V konfiguraci zástupného textu operaci nakonfigurujeme řídicí panel prostředků do mezipaměti alespoň jednu hodinu, protože jsme pochopení povahy dat znamená, že i když je zastaralý, že budou mít pořád dostatečně efektivní ke sdělení cenné informace pro uživatele hodinu.</span><span class="sxs-lookup"><span data-stu-id="6b822-187">In the configuration of the placeholder operation we can configure the dashboard resource to be cached for at least an hour because we understand the nature of the data means that even if it is an hour out of date, it will still be sufficiently effective to convey valuable information to the users.</span></span>

## <a name="summary"></a><span data-ttu-id="6b822-188">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6b822-188">Summary</span></span>
<span data-ttu-id="6b822-189">Služba Azure API Management poskytuje flexibilní zásady, které je možné selektivně použít pro provoz protokolu HTTP a umožňuje složení služeb back-end.</span><span class="sxs-lookup"><span data-stu-id="6b822-189">Azure API Management service provides flexible policies that can be selectively applied to HTTP traffic and enables composition of backend services.</span></span> <span data-ttu-id="6b822-190">Jestli chcete vylepšit bránu rozhraní API s výstrahy funkce, ověření, možnosti ověření nebo vytvořit nové složené prostředky v závislosti na více služeb back-end `send-request` a související zásady otevřete široké možnosti.</span><span class="sxs-lookup"><span data-stu-id="6b822-190">Whether you want to enhance your API gateway with alerting functions, verification, validation capabilities or create new composite resources based on multiple backend services, the `send-request` and related policies open a world of possibilities.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="6b822-191">Podívejte se na video s přehledem těchto zásad</span><span class="sxs-lookup"><span data-stu-id="6b822-191">Watch a video overview of these policies</span></span>
<span data-ttu-id="6b822-192">Další informace o [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), a [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásady popsaná v tomto článku prosím v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="6b822-192">For more information on the [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), and [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policies covered in this article, please watch the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

