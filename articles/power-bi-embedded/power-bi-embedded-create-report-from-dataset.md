---
title: "Vytvoření nové sestavy z datové sady v Azure Power BI Embedded | Microsoft Docs"
description: "Power BI Embedded sestavy lze vytvořit nyní z datové sady ve vaší vlastní aplikaci."
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
ms.openlocfilehash: 457f53aa76059dbb2faed6b264102f1f59b9918a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="8d4c1-103">Vytvoření nové sestavy z datové sady v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="8d4c1-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="8d4c1-104">Power BI Embedded sestavy lze vytvořit nyní z datové sady ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="8d4c1-105">Metoda ověřování je podobný že sestavy vložit.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-105">The authentication method is similar to that of report embed.</span></span> <span data-ttu-id="8d4c1-106">Je založena na přístupové tokeny, které jsou specifické pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-106">It is based on access tokens that are specific to a dataset.</span></span> <span data-ttu-id="8d4c1-107">Pomocí Azure Active Directory (AAD) jsou vydávány tokeny, na které se používá pro PowerBI.com a vlastní službou jsou vydávány tokeny Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="8d4c1-108">Když jsou createing vložených sestavy, tokeny vydané pro konkrétní datové sady.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-108">When createing an Embedded report, the tokens issued are for a specific dataset.</span></span> <span data-ttu-id="8d4c1-109">Tokeny by měly být přidruženy s adresou URL vložení u stejného elementu k zajistěte, aby byl každý token jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-109">Tokens should be associated with the embed URL on the same element to ensure each has a unique token.</span></span> <span data-ttu-id="8d4c1-110">Chcete-li vytvořit sestavu vložených *Dataset.Read a Workspace.Report.Create* obory musí být zadáno v tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-110">In order to create an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in the access token.</span></span>

## <a name="create-access-token-needed-to-create-new-report"></a><span data-ttu-id="8d4c1-111">Vytvoření tokenu přístupu, které jsou nutné k vytvoření nové sestavy</span><span class="sxs-lookup"><span data-stu-id="8d4c1-111">Create access token needed to create new report</span></span>

<span data-ttu-id="8d4c1-112">Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="8d4c1-113">Přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů jsou podepsané tokeny.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-113">The tokens are signed with the access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="8d4c1-114">Vložení tokeny, ve výchozím nastavení, používají se pro čtení pouze přístup k sestavě pro vložení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-114">Embed tokens, by default, are used to provide read only access to a report to embed into an application.</span></span> <span data-ttu-id="8d4c1-115">Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="8d4c1-116">Přístupové tokeny by se vytvořit na serveru, přístupové klíče se používají pro přihlášení nebo šifrování tokenů.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-116">Access tokens should be created on the server as the access keys are used to sign/encrypt the tokens.</span></span> <span data-ttu-id="8d4c1-117">Informace o tom, jak vytvořit token přístupu najdete v tématu [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="8d4c1-117">For information on how to create an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="8d4c1-118">Můžete také zkontrolovat [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-118">You can also review the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="8d4c1-119">Tady je příklad co to může vypadat třeba pomocí sady .NET SDK pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-119">Here is an example of what this would look like using the .NET SDK for Power BI.</span></span>

<span data-ttu-id="8d4c1-120">V tomto příkladu máme naše id sady dat, který chceme creat – nové sestavy na.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-120">In this example, we have our dataset id that we want to creat the new report on.</span></span> <span data-ttu-id="8d4c1-121">Také je potřeba přidat obory *Dataset.Read a Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-121">We also need to add the scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="8d4c1-122">*PowerBIToken třída* je nutné nainstalovat [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="8d4c1-122">The *PowerBIToken class* requires that you install the [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="8d4c1-123">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="8d4c1-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="8d4c1-124">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="8d4c1-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="8d4c1-125">Vytvoření nové prázdné sestavy</span><span class="sxs-lookup"><span data-stu-id="8d4c1-125">Create a new blank report</span></span>

<span data-ttu-id="8d4c1-126">Chcete-li vytvořit novou sestavu, je třeba poskytnout vytvořit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-126">In order to create a new report, the create configuration should be provided.</span></span> <span data-ttu-id="8d4c1-127">To by mělo zahrnovat přístupový token, embedURL a datasetID, který chcete vytvořit sestavu s.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-127">This should include the access token, the embedURL and the datasetID that we want to create the report against.</span></span> <span data-ttu-id="8d4c1-128">To je nutné nainstalovat nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="8d4c1-128">This requires that you install the nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="8d4c1-129">EmbedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-129">The embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="8d4c1-130">Můžete použít [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) správnou funkci.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-130">You can use the [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) to test functionality.</span></span> <span data-ttu-id="8d4c1-131">Také nabízí příklady kódu pro různé operace, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-131">It also gives code examples for the different operations that are available.</span></span>

<span data-ttu-id="8d4c1-132">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="8d4c1-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="8d4c1-133">**Kód jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="8d4c1-133">**JavaScript code**</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

<span data-ttu-id="8d4c1-134">Volání metody *powerbi.createReport()* bude prázdného plátna v režimu úprav zobrazí v rámci *div* elementu.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within the *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="8d4c1-135">Uložit nové sestavy</span><span class="sxs-lookup"><span data-stu-id="8d4c1-135">Save new reports</span></span>

<span data-ttu-id="8d4c1-136">Sestava nebude vytvořena ve skutečnosti, dokud zavoláte **uložit jako** operaci.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-136">The report will not actually be created until you call the **save as** operation.</span></span> <span data-ttu-id="8d4c1-137">Tento krok můžete provést z nabídky soubor nebo pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-137">This can be done from file menu or from JavaScript.</span></span>

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> <span data-ttu-id="8d4c1-138">Vytvoření nové sestavy až po **uložit jako** je volána.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="8d4c1-139">Po uložení se na plátno, bude mít datovou sadu v režimu úprav a není sestava.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-139">After you save, the canvas will still show the dataset in edit mode and not the report.</span></span> <span data-ttu-id="8d4c1-140">Musíte se znovu načíst nové sestavy, stejně jako u jiných sestavách.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-140">You will need to reload the new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a><span data-ttu-id="8d4c1-141">Načtení nové sestavy</span><span class="sxs-lookup"><span data-stu-id="8d4c1-141">Load the new report</span></span>

<span data-ttu-id="8d4c1-142">Aby bylo možné využívat nové sestavy, které potřebujete k vložení stejným způsobem, jakým aplikace vloží regulární sestavy, což znamená, nový token musí být vydaný speciálně pro novou sestavu a pak volat metodu vložení.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-142">In order to interact with the new report you need to embed it in the same way the application embeds a regular report, meaning, a new token must be issued specifically for the new report and then call the embed method.</span></span>

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a><span data-ttu-id="8d4c1-143">Automatizovat uložení a načtení nové sestavy pomocí "uložené" události</span><span class="sxs-lookup"><span data-stu-id="8d4c1-143">Automate save and load of a new report using the "saved" event</span></span>

<span data-ttu-id="8d4c1-144">V pořadí pro automatizaci procesu "Uložit jako" a pak načítání nové sestavy, které lze provést použitím "uložené" události.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-144">In order to automate the process of "save as" and then loading the new report you can make use of the "saved" Event.</span></span> <span data-ttu-id="8d4c1-145">Tato událost je aktivována, pokud uložení bylo dokončeno a vrátí objekt Json obsahující nové reportId, název sestavy, staré reportId (pokud existuje) a pokud byla operace Uložit jako nebo uložit.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-145">This event is fired when the save operation is complete and it returns a Json object containing the new reportId, report name, the old reportId(if there was one) and if the operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="8d4c1-146">Pro automatizaci procesu můžete naslouchat "uložené" události, proveďte nové reportId, vytvořit nový token a vložit novou sestavu s ním.</span><span class="sxs-lookup"><span data-stu-id="8d4c1-146">To Automate the process you can listen on the "saved" event, take the new reportId, create the new token and embed the new report with it.</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
```

## <a name="see-also"></a><span data-ttu-id="8d4c1-147">Viz také</span><span class="sxs-lookup"><span data-stu-id="8d4c1-147">See also</span></span>

[<span data-ttu-id="8d4c1-148">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="8d4c1-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="8d4c1-149">Ukládání sestav</span><span class="sxs-lookup"><span data-stu-id="8d4c1-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="8d4c1-150">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="8d4c1-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="8d4c1-151">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="8d4c1-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="8d4c1-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="8d4c1-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="8d4c1-153">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="8d4c1-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="8d4c1-154">Power BI základní NuGut balíčku</span><span class="sxs-lookup"><span data-stu-id="8d4c1-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="8d4c1-155">Power BI JavaScript balíčku</span><span class="sxs-lookup"><span data-stu-id="8d4c1-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="8d4c1-156">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="8d4c1-156">More questions?</span></span> [<span data-ttu-id="8d4c1-157">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="8d4c1-157">Try the Power BI Community</span></span>](http://community.powerbi.com/)