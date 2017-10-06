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
# <a name="embed-a-report-in-power-bi-embedded"></a><span data-ttu-id="ba48e-103">Vložení sestavy v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="ba48e-103">Embed a report in Power BI Embedded</span></span>

<span data-ttu-id="ba48e-104">Zjistěte, jak tooembed sestava, která je v Power BI Embedded do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba48e-104">Learn how tooembed a report that is in Power BI Embedded into your application.</span></span>

<span data-ttu-id="ba48e-105">Podíváme se na tom, jak tooactually vložení sestavy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba48e-105">We will look at how tooactually embed a report into your application.</span></span> <span data-ttu-id="ba48e-106">Toto předpokládá, že už máte sestavu, která existuje v rámci pracovního prostoru v kolekci pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ba48e-106">This is assuming you already have a report that exists within a workspace in your workspace collection.</span></span> <span data-ttu-id="ba48e-107">Pokud tento krok jste ještě neudělali, přečtěte si téma [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba48e-107">If you haven't done that step yet, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

<span data-ttu-id="ba48e-108">Můžete použít hello .NET (C#) nebo Node.js SDK, společně s JavaScript, tooeasily sestavení vaší aplikace pomocí Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="ba48e-108">You can use hello .NET (C#) or Node.js SDK, along with JavaScript, tooeasily build your application with Power BI Embedded.</span></span> 

## <a name="using-hello-access-keys-toouse-rest-apis"></a><span data-ttu-id="ba48e-109">Pomocí hello přístup klíče toouse rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="ba48e-109">Using hello access keys toouse REST APIs</span></span>

<span data-ttu-id="ba48e-110">V pořadí toocall hello REST API které lze předat hello přístupový klíč, který můžete získat z hello portál Azure pro kolekci daného pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ba48e-110">In order toocall hello REST API, you can pass hello access key which you can get from hello Azure portal for a given workspace collection.</span></span> <span data-ttu-id="ba48e-111">Další informace najdete v tématu [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba48e-111">For more information, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="get-a-report-id"></a><span data-ttu-id="ba48e-112">Získání id sestavy</span><span class="sxs-lookup"><span data-stu-id="ba48e-112">Get a report id</span></span>

<span data-ttu-id="ba48e-113">Každý přístupový token je založena na sestavu.</span><span class="sxs-lookup"><span data-stu-id="ba48e-113">Every access token is based on a report.</span></span> <span data-ttu-id="ba48e-114">Budete potřebovat hello tooget pro hello sestavy, které chcete tooembed zadané id sestavy.</span><span class="sxs-lookup"><span data-stu-id="ba48e-114">You will need tooget hello given report id for hello report that you want tooembed.</span></span> <span data-ttu-id="ba48e-115">To lze provést podle volání toohello [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="ba48e-115">This can be done based on calls toohello [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span></span> <span data-ttu-id="ba48e-116">Tato možnost vrátí hello sestavy id a hello vložte adresu url.</span><span class="sxs-lookup"><span data-stu-id="ba48e-116">This will return hello report id and hello embed url.</span></span> <span data-ttu-id="ba48e-117">To lze provést pomocí hello Power BI .NET SDK nebo přímé volání rozhraní REST API hello.</span><span class="sxs-lookup"><span data-stu-id="ba48e-117">This can be done using hello Power BI .NET SDK or calling hello REST API directly.</span></span>

### <a name="using-hello-power-bi-net-sdk"></a><span data-ttu-id="ba48e-118">Pomocí hello Power BI .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ba48e-118">Using hello Power BI .NET SDK</span></span>

<span data-ttu-id="ba48e-119">Pokud používáte hello .NET SDK, budete potřebovat toocreate tokenu přihlašovacích údajů, která je založena na hello přístupový klíč, který můžete získat z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ba48e-119">When using hello .NET SDK, you will need toocreate a token credential which is based on hello access key you get from hello Azure portal.</span></span> <span data-ttu-id="ba48e-120">To je nutné nainstalovat hello [balíček NuGet pro rozhraní API Power BI](https://www.nuget.org/profiles/powerbi).</span><span class="sxs-lookup"><span data-stu-id="ba48e-120">This requires that you install hello [Power BI API NuGet package](https://www.nuget.org/profiles/powerbi).</span></span>

<span data-ttu-id="ba48e-121">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="ba48e-121">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Api
```

<span data-ttu-id="ba48e-122">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="ba48e-122">**C# code**</span></span>

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a><span data-ttu-id="ba48e-123">Volání hello přímo REST API</span><span class="sxs-lookup"><span data-stu-id="ba48e-123">Calling hello REST API directly</span></span>

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

## <a name="create-an-access-token"></a><span data-ttu-id="ba48e-124">Vytvořit token přístupu</span><span class="sxs-lookup"><span data-stu-id="ba48e-124">Create an access token</span></span>

<span data-ttu-id="ba48e-125">Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON.</span><span class="sxs-lookup"><span data-stu-id="ba48e-125">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="ba48e-126">Hello tokeny jsou podepsané hello přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="ba48e-126">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="ba48e-127">Vložení tokeny, ve výchozím nastavení, jsou použité tooprovide číst přístup jenom k tooembed tooa sestavy do aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba48e-127">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="ba48e-128">Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.</span><span class="sxs-lookup"><span data-stu-id="ba48e-128">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="ba48e-129">Přístupové tokeny musí být vytvořena na serveru hello hello přístupové klíče jsou použité toosign nebo šifrování tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="ba48e-129">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="ba48e-130">Informace o tom najdete v části toocreate přístupový token, [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="ba48e-130">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="ba48e-131">Můžete také zkontrolovat hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda.</span><span class="sxs-lookup"><span data-stu-id="ba48e-131">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="ba48e-132">Tady je příklad co to může vypadat třeba pomocí hello .NET SDK pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="ba48e-132">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="ba48e-133">Použijete id hello sestavy, které jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="ba48e-133">You will use hello report id that you retrieved earlier.</span></span> <span data-ttu-id="ba48e-134">Po vložení hello je vytvořen token, pak bude používat hello přístupový klíč toogenerate hello token, který můžete použít z pohledu hello javascript.</span><span class="sxs-lookup"><span data-stu-id="ba48e-134">Once hello embed token is created, you will then use hello access key toogenerate hello token which you can use from hello javascript perspective.</span></span> <span data-ttu-id="ba48e-135">Hello *PowerBIToken třída* vyžaduje instalaci hello [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="ba48e-135">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="ba48e-136">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="ba48e-136">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="ba48e-137">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="ba48e-137">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a><span data-ttu-id="ba48e-138">Přidání oprávnění obory tooembed tokenů</span><span class="sxs-lookup"><span data-stu-id="ba48e-138">Adding permission scopes tooembed tokens</span></span>

<span data-ttu-id="ba48e-139">Pokud používáte vložení tokeny, může být vhodné toorestrict využití hello prostředků, které vám umožní získat přístup k.</span><span class="sxs-lookup"><span data-stu-id="ba48e-139">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="ba48e-140">Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ba48e-140">For this reason, you can generate a token with scoped permissions.</span></span> <span data-ttu-id="ba48e-141">Další informace najdete v tématu [oborů](power-bi-embedded-app-token-flow.md#scopes)</span><span class="sxs-lookup"><span data-stu-id="ba48e-141">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes)</span></span>

## <a name="embed-using-javascript"></a><span data-ttu-id="ba48e-142">Vložení pomocí jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ba48e-142">Embed using JavaScript</span></span>

<span data-ttu-id="ba48e-143">Až budete mít hello přístupový token a id hello sestavy, jsme vložení sestavy hello pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ba48e-143">After you have hello access token and hello report id, we can embed hello report using JavaScript.</span></span> <span data-ttu-id="ba48e-144">To je nutné nainstalovat hello nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="ba48e-144">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="ba48e-145">Hello embedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="ba48e-145">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="ba48e-146">Můžete použít hello [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkce.</span><span class="sxs-lookup"><span data-stu-id="ba48e-146">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="ba48e-147">Také nabízí příklady kódu pro hello různé operace, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ba48e-147">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="ba48e-148">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="ba48e-148">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="ba48e-149">**Kód jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="ba48e-149">**JavaScript code**</span></span>

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

### <a name="set-hello-size-of-embedded-elements"></a><span data-ttu-id="ba48e-150">Nastavení velikosti hello vložených elementů</span><span class="sxs-lookup"><span data-stu-id="ba48e-150">Set hello size of embedded elements</span></span>

<span data-ttu-id="ba48e-151">Sestava Hello automaticky vložený závislosti na velikosti hello svého kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ba48e-151">hello report will automatically be embedded based on hello size of its container.</span></span> <span data-ttu-id="ba48e-152">Výchozí velikost toooverride hello hello vloží jednoduše přidat třídy atribut nebo vložené stylů CSS pro šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="ba48e-152">toooverride hello default size of hello embeds simply add a CSS class attribute or inline styles for width & height.</span></span>

## <a name="see-also"></a><span data-ttu-id="ba48e-153">Viz také</span><span class="sxs-lookup"><span data-stu-id="ba48e-153">See also</span></span>

[<span data-ttu-id="ba48e-154">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="ba48e-154">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="ba48e-155">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="ba48e-155">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="ba48e-156">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="ba48e-156">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="ba48e-157">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="ba48e-157">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="ba48e-158">Power BI JavaScript balíčku</span><span class="sxs-lookup"><span data-stu-id="ba48e-158">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="ba48e-159">[Power BI rozhraní API NuGet balíček](https://www.nuget.org/profiles/powerbi)
[Power BI základní NuGut balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span><span class="sxs-lookup"><span data-stu-id="ba48e-159">[Power BI API NuGet package](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span></span>  
[<span data-ttu-id="ba48e-160">Úložiště Git PowerBI CSharp</span><span class="sxs-lookup"><span data-stu-id="ba48e-160">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="ba48e-161">Úložiště Git PowerBI uzlu</span><span class="sxs-lookup"><span data-stu-id="ba48e-161">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="ba48e-162">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="ba48e-162">More questions?</span></span> [<span data-ttu-id="ba48e-163">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="ba48e-163">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
