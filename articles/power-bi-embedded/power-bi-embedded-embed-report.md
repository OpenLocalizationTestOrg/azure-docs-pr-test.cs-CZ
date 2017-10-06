---
title: aaaEmbed sestavy v Azure Power BI Embedded | Microsoft Docs
description: "Zjistěte, jak tooembed sestava, která je v Power BI Embedded do vaší aplikace."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: f25344bbd0b9c092ef19da04d0b455d453b426a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="embed-a-report-in-power-bi-embedded"></a>Vložení sestavy v Power BI Embedded

Zjistěte, jak tooembed sestava, která je v Power BI Embedded do vaší aplikace.

Podíváme se na tom, jak tooactually vložení sestavy do vaší aplikace. Toto předpokládá, že už máte sestavu, která existuje v rámci pracovního prostoru v kolekci pracovního prostoru. Pokud tento krok jste ještě neudělali, přečtěte si téma [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

Můžete použít hello .NET (C#) nebo Node.js SDK, společně s JavaScript, tooeasily sestavení vaší aplikace pomocí Power BI Embedded. 

## <a name="using-hello-access-keys-toouse-rest-apis"></a>Pomocí hello přístup klíče toouse rozhraní REST API

V pořadí toocall hello REST API které lze předat hello přístupový klíč, který můžete získat z hello portál Azure pro kolekci daného pracovního prostoru. Další informace najdete v tématu [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="get-a-report-id"></a>Získání id sestavy

Každý přístupový token je založena na sestavu. Budete potřebovat hello tooget pro hello sestavy, které chcete tooembed zadané id sestavy. To lze provést podle volání toohello [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API. Tato možnost vrátí hello sestavy id a hello vložte adresu url. To lze provést pomocí hello Power BI .NET SDK nebo přímé volání rozhraní REST API hello.

### <a name="using-hello-power-bi-net-sdk"></a>Pomocí hello Power BI .NET SDK

Pokud používáte hello .NET SDK, budete potřebovat toocreate tokenu přihlašovacích údajů, která je založena na hello přístupový klíč, který můžete získat z hello portálu Azure. To je nutné nainstalovat hello [balíček NuGet pro rozhraní API Power BI](https://www.nuget.org/profiles/powerbi).

**Instalace balíčku NuGet**

```
Install-Package Microsoft.PowerBI.Api
```

**Kód jazyka C#**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a>Volání hello přímo REST API

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response tooget hello report you want toowork with.
    }

}
```

## <a name="create-an-access-token"></a>Vytvořit token přístupu

Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON. Hello tokeny jsou podepsané hello přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů. Vložení tokeny, ve výchozím nastavení, jsou použité tooprovide číst přístup jenom k tooembed tooa sestavy do aplikace. Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.

Přístupové tokeny musí být vytvořena na serveru hello hello přístupové klíče jsou použité toosign nebo šifrování tokenů hello. Informace o tom najdete v části toocreate přístupový token, [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md). Můžete také zkontrolovat hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda. Tady je příklad co to může vypadat třeba pomocí hello .NET SDK pro Power BI.

Použijete id hello sestavy, které jste získali dříve. Po vložení hello je vytvořen token, pak bude používat hello přístupový klíč toogenerate hello token, který můžete použít z pohledu hello javascript. Hello *PowerBIToken třída* vyžaduje instalaci hello [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**Instalace balíčku NuGet**

```
Install-Package Microsoft.PowerBI.Core
```

**Kód jazyka C#**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a>Přidání oprávnění obory tooembed tokenů

Pokud používáte vložení tokeny, může být vhodné toorestrict využití hello prostředků, které vám umožní získat přístup k. Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění. Další informace najdete v tématu [oborů](power-bi-embedded-app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Vložení pomocí jazyka JavaScript

Až budete mít hello přístupový token a id hello sestavy, jsme vložení sestavy hello pomocí jazyka JavaScript. To je nutné nainstalovat hello nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). Hello embedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Můžete použít hello [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkce. Také nabízí příklady kódu pro hello různé operace, které jsou k dispozici.

**Instalace balíčku NuGet**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**Kód jazyka JavaScript**

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-hello-size-of-embedded-elements"></a>Nastavení velikosti hello vložených elementů

Sestava Hello automaticky vložený závislosti na velikosti hello svého kontejneru. Výchozí velikost toooverride hello hello vloží jednoduše přidat třídy atribut nebo vložené stylů CSS pro šířku a výšku.

## <a name="see-also"></a>Viz také

[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI rozhraní API NuGet balíček](https://www.nuget.org/profiles/powerbi)
[Power BI základní NuGut balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Úložiště Git PowerBI CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Úložiště Git PowerBI uzlu](https://github.com/Microsoft/PowerBI-Node)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)
