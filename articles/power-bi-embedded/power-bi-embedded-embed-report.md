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
# <a name="embed-a-report-in-power-bi-embedded"></a><span data-ttu-id="057c7-103">Vložení sestavy v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="057c7-103">Embed a report in Power BI Embedded</span></span>

<span data-ttu-id="057c7-104">Postup vložení sestavy, který je v Power BI Embedded do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="057c7-104">Learn how to embed a report that is in Power BI Embedded into your application.</span></span>

<span data-ttu-id="057c7-105">Podíváme se na tom, jak ve skutečnosti vložení sestavy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="057c7-105">We will look at how to actually embed a report into your application.</span></span> <span data-ttu-id="057c7-106">Toto předpokládá, že už máte sestavu, která existuje v rámci pracovního prostoru v kolekci pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="057c7-106">This is assuming you already have a report that exists within a workspace in your workspace collection.</span></span> <span data-ttu-id="057c7-107">Pokud tento krok jste ještě neudělali, přečtěte si téma [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="057c7-107">If you haven't done that step yet, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

<span data-ttu-id="057c7-108">Rozhraní .NET (C#) nebo Node.js SDK, společně s JavaScript, můžete snadno vytvářet aplikace s Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="057c7-108">You can use the .NET (C#) or Node.js SDK, along with JavaScript, to easily build your application with Power BI Embedded.</span></span> 

## <a name="using-the-access-keys-to-use-rest-apis"></a><span data-ttu-id="057c7-109">Pomocí rozhraní REST API pomocí přístupových kláves</span><span class="sxs-lookup"><span data-stu-id="057c7-109">Using the access keys to use REST APIs</span></span>

<span data-ttu-id="057c7-110">Aby bylo možné volat rozhraní REST API, můžete předat přístupový klíč, který můžete získat z portálu Azure pro kolekci daného pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="057c7-110">In order to call the REST API, you can pass the access key which you can get from the Azure portal for a given workspace collection.</span></span> <span data-ttu-id="057c7-111">Další informace najdete v tématu [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="057c7-111">For more information, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="get-a-report-id"></a><span data-ttu-id="057c7-112">Získání id sestavy</span><span class="sxs-lookup"><span data-stu-id="057c7-112">Get a report id</span></span>

<span data-ttu-id="057c7-113">Každý přístupový token je založena na sestavu.</span><span class="sxs-lookup"><span data-stu-id="057c7-113">Every access token is based on a report.</span></span> <span data-ttu-id="057c7-114">Musíte získat id dané sestavy pro sestavu, kterou chcete vložit.</span><span class="sxs-lookup"><span data-stu-id="057c7-114">You will need to get the given report id for the report that you want to embed.</span></span> <span data-ttu-id="057c7-115">To lze provést na základě volání [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="057c7-115">This can be done based on calls to the [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span></span> <span data-ttu-id="057c7-116">Tato možnost vrátí id sestavy a adresu url pro vložení.</span><span class="sxs-lookup"><span data-stu-id="057c7-116">This will return the report id and the embed url.</span></span> <span data-ttu-id="057c7-117">To lze provést pomocí .NET SDK služby Power BI nebo přímé volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="057c7-117">This can be done using the Power BI .NET SDK or calling the REST API directly.</span></span>

### <a name="using-the-power-bi-net-sdk"></a><span data-ttu-id="057c7-118">Pomocí .NET SDK služby Power BI</span><span class="sxs-lookup"><span data-stu-id="057c7-118">Using the Power BI .NET SDK</span></span>

<span data-ttu-id="057c7-119">Když pomocí sady .NET SDK, musíte se k vytvoření tokenu přihlašovacích údajů, která je založena na přístupový klíč, který můžete získat z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="057c7-119">When using the .NET SDK, you will need to create a token credential which is based on the access key you get from the Azure portal.</span></span> <span data-ttu-id="057c7-120">To je nutné nainstalovat [balíček NuGet pro rozhraní API Power BI](https://www.nuget.org/profiles/powerbi).</span><span class="sxs-lookup"><span data-stu-id="057c7-120">This requires that you install the [Power BI API NuGet package](https://www.nuget.org/profiles/powerbi).</span></span>

<span data-ttu-id="057c7-121">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="057c7-121">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Api
```

<span data-ttu-id="057c7-122">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="057c7-122">**C# code**</span></span>

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select the report that you want to work with from the collection of reports.
```

### <a name="calling-the-rest-api-directly"></a><span data-ttu-id="057c7-123">Přímé volání rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="057c7-123">Calling the REST API directly</span></span>

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

## <a name="create-an-access-token"></a><span data-ttu-id="057c7-124">Vytvořit token přístupu</span><span class="sxs-lookup"><span data-stu-id="057c7-124">Create an access token</span></span>

<span data-ttu-id="057c7-125">Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON.</span><span class="sxs-lookup"><span data-stu-id="057c7-125">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="057c7-126">Přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů jsou podepsané tokeny.</span><span class="sxs-lookup"><span data-stu-id="057c7-126">The tokens are signed with the access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="057c7-127">Vložení tokeny, ve výchozím nastavení, používají se pro čtení pouze přístup k sestavě pro vložení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="057c7-127">Embed tokens, by default, are used to provide read only access to a report to embed into an application.</span></span> <span data-ttu-id="057c7-128">Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.</span><span class="sxs-lookup"><span data-stu-id="057c7-128">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="057c7-129">Přístupové tokeny by se vytvořit na serveru, přístupové klíče se používají pro přihlášení nebo šifrování tokenů.</span><span class="sxs-lookup"><span data-stu-id="057c7-129">Access tokens should be created on the server as the access keys are used to sign/encrypt the tokens.</span></span> <span data-ttu-id="057c7-130">Informace o tom, jak vytvořit token přístupu najdete v tématu [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="057c7-130">For information on how to create an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="057c7-131">Můžete také zkontrolovat [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda.</span><span class="sxs-lookup"><span data-stu-id="057c7-131">You can also review the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="057c7-132">Tady je příklad co to může vypadat třeba pomocí sady .NET SDK pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="057c7-132">Here is an example of what this would look like using the .NET SDK for Power BI.</span></span>

<span data-ttu-id="057c7-133">Použijete id sestavy, které jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="057c7-133">You will use the report id that you retrieved earlier.</span></span> <span data-ttu-id="057c7-134">Po vytvoření vkládací token pak použijete přístupový klíč pro vygenerování tokenu, který můžete použít z pohledu javascript.</span><span class="sxs-lookup"><span data-stu-id="057c7-134">Once the embed token is created, you will then use the access key to generate the token which you can use from the javascript perspective.</span></span> <span data-ttu-id="057c7-135">*PowerBIToken třída* je nutné nainstalovat [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="057c7-135">The *PowerBIToken class* requires that you install the [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="057c7-136">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="057c7-136">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="057c7-137">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="057c7-137">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-to-embed-tokens"></a><span data-ttu-id="057c7-138">Přidání obory oprávnění pro vložení tokeny</span><span class="sxs-lookup"><span data-stu-id="057c7-138">Adding permission scopes to embed tokens</span></span>

<span data-ttu-id="057c7-139">Pokud používáte vložení tokeny, můžete omezit využití prostředků, které vám umožní získat přístup k.</span><span class="sxs-lookup"><span data-stu-id="057c7-139">When using Embed tokens, you may want to restrict usage of the resources you give access to.</span></span> <span data-ttu-id="057c7-140">Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.</span><span class="sxs-lookup"><span data-stu-id="057c7-140">For this reason, you can generate a token with scoped permissions.</span></span> <span data-ttu-id="057c7-141">Další informace najdete v tématu [oborů](power-bi-embedded-app-token-flow.md#scopes)</span><span class="sxs-lookup"><span data-stu-id="057c7-141">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes)</span></span>

## <a name="embed-using-javascript"></a><span data-ttu-id="057c7-142">Vložení pomocí jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="057c7-142">Embed using JavaScript</span></span>

<span data-ttu-id="057c7-143">Až budete mít přístupový token a id sestavy, jsme vložení sestavy pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="057c7-143">After you have the access token and the report id, we can embed the report using JavaScript.</span></span> <span data-ttu-id="057c7-144">To je nutné nainstalovat nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="057c7-144">This requires that you install the nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="057c7-145">EmbedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="057c7-145">The embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="057c7-146">Můžete použít [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) správnou funkci.</span><span class="sxs-lookup"><span data-stu-id="057c7-146">You can use the [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) to test functionality.</span></span> <span data-ttu-id="057c7-147">Také nabízí příklady kódu pro různé operace, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="057c7-147">It also gives code examples for the different operations that are available.</span></span>

<span data-ttu-id="057c7-148">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="057c7-148">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="057c7-149">**Kód jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="057c7-149">**JavaScript code**</span></span>

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

### <a name="set-the-size-of-embedded-elements"></a><span data-ttu-id="057c7-150">Nastavení velikosti vložené prvky</span><span class="sxs-lookup"><span data-stu-id="057c7-150">Set the size of embedded elements</span></span>

<span data-ttu-id="057c7-151">Sestava bude automaticky vložený podle velikosti svého kontejneru.</span><span class="sxs-lookup"><span data-stu-id="057c7-151">The report will automatically be embedded based on the size of its container.</span></span> <span data-ttu-id="057c7-152">Chcete-li přepsat výchozí velikost vloží jednoduše přidat třídy atribut nebo vložené stylů CSS pro šířku a výšku.</span><span class="sxs-lookup"><span data-stu-id="057c7-152">To override the default size of the embeds simply add a CSS class attribute or inline styles for width & height.</span></span>

## <a name="see-also"></a><span data-ttu-id="057c7-153">Viz také</span><span class="sxs-lookup"><span data-stu-id="057c7-153">See also</span></span>

[<span data-ttu-id="057c7-154">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="057c7-154">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="057c7-155">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="057c7-155">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="057c7-156">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="057c7-156">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="057c7-157">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="057c7-157">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="057c7-158">Power BI JavaScript balíčku</span><span class="sxs-lookup"><span data-stu-id="057c7-158">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="057c7-159">[Power BI rozhraní API NuGet balíček](https://www.nuget.org/profiles/powerbi)
[Power BI základní NuGut balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span><span class="sxs-lookup"><span data-stu-id="057c7-159">[Power BI API NuGet package](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span></span>  
[<span data-ttu-id="057c7-160">Úložiště Git PowerBI CSharp</span><span class="sxs-lookup"><span data-stu-id="057c7-160">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="057c7-161">Úložiště Git PowerBI uzlu</span><span class="sxs-lookup"><span data-stu-id="057c7-161">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="057c7-162">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="057c7-162">More questions?</span></span> [<span data-ttu-id="057c7-163">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="057c7-163">Try the Power BI Community</span></span>](http://community.powerbi.com/)
