---
title: "Vložení sestavy v Azure Power BI Embedded | Microsoft Docs"
description: "Postup vložení sestavy, který je v Power BI Embedded do vaší aplikace."
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
ms.openlocfilehash: 3d865af2418c9c557c861a379766a125d75cebf1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="embed-a-report-in-power-bi-embedded"></a>Vložení sestavy v Power BI Embedded

Postup vložení sestavy, který je v Power BI Embedded do vaší aplikace.

Podíváme se na tom, jak ve skutečnosti vložení sestavy do vaší aplikace. Toto předpokládá, že už máte sestavu, která existuje v rámci pracovního prostoru v kolekci pracovního prostoru. Pokud tento krok jste ještě neudělali, přečtěte si téma [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

Rozhraní .NET (C#) nebo Node.js SDK, společně s JavaScript, můžete snadno vytvářet aplikace s Power BI Embedded. 

## <a name="using-the-access-keys-to-use-rest-apis"></a>Pomocí rozhraní REST API pomocí přístupových kláves

Aby bylo možné volat rozhraní REST API, můžete předat přístupový klíč, který můžete získat z portálu Azure pro kolekci daného pracovního prostoru. Další informace najdete v tématu [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="get-a-report-id"></a>Získání id sestavy

Každý přístupový token je založena na sestavu. Musíte získat id dané sestavy pro sestavu, kterou chcete vložit. To lze provést na základě volání [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API. Tato možnost vrátí id sestavy a adresu url pro vložení. To lze provést pomocí .NET SDK služby Power BI nebo přímé volání rozhraní REST API.

### <a name="using-the-power-bi-net-sdk"></a>Pomocí .NET SDK služby Power BI

Když pomocí sady .NET SDK, musíte se k vytvoření tokenu přihlašovacích údajů, která je založena na přístupový klíč, který můžete získat z portálu Azure. To je nutné nainstalovat [balíček NuGet pro rozhraní API Power BI](https://www.nuget.org/profiles/powerbi).

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

// Select the report that you want to work with from the collection of reports.
```

### <a name="calling-the-rest-api-directly"></a>Přímé volání rozhraní REST API

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response to get the report you want to work with.
    }

}
```

## <a name="create-an-access-token"></a>Vytvořit token přístupu

Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON. Přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů jsou podepsané tokeny. Vložení tokeny, ve výchozím nastavení, používají se pro čtení pouze přístup k sestavě pro vložení do aplikace. Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.

Přístupové tokeny by se vytvořit na serveru, přístupové klíče se používají pro přihlášení nebo šifrování tokenů. Informace o tom, jak vytvořit token přístupu najdete v tématu [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md). Můžete také zkontrolovat [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda. Tady je příklad co to může vypadat třeba pomocí sady .NET SDK pro Power BI.

Použijete id sestavy, které jste získali dříve. Po vytvoření vkládací token pak použijete přístupový klíč pro vygenerování tokenu, který můžete použít z pohledu javascript. *PowerBIToken třída* je nutné nainstalovat [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

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

### <a name="adding-permission-scopes-to-embed-tokens"></a>Přidání obory oprávnění pro vložení tokeny

Pokud používáte vložení tokeny, můžete omezit využití prostředků, které vám umožní získat přístup k. Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění. Další informace najdete v tématu [oborů](power-bi-embedded-app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Vložení pomocí jazyka JavaScript

Až budete mít přístupový token a id sestavy, jsme vložení sestavy pomocí jazyka JavaScript. To je nutné nainstalovat nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). EmbedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Můžete použít [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) správnou funkci. Také nabízí příklady kódu pro různé operace, které jsou k dispozici.

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

### <a name="set-the-size-of-embedded-elements"></a>Nastavení velikosti vložené prvky

Sestava bude automaticky vložený podle velikosti svého kontejneru. Chcete-li přepsat výchozí velikost vloží jednoduše přidat třídy atribut nebo vložené stylů CSS pro šířku a výšku.

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
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)
