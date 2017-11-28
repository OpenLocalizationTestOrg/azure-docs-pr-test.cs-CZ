---
title: "aaaCreate nové sestavy z datové sady v Azure Power BI Embedded | Microsoft Docs"
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
ms.openlocfilehash: 41a0a52e4c833313f495bb5ff14749203fef9b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="3e467-103">Vytvoření nové sestavy z datové sady v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="3e467-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="3e467-104">Power BI Embedded sestavy lze vytvořit nyní z datové sady ve vaší vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e467-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="3e467-105">Hello metoda ověřování je podobný toothat sestavy pro vložení.</span><span class="sxs-lookup"><span data-stu-id="3e467-105">hello authentication method is similar toothat of report embed.</span></span> <span data-ttu-id="3e467-106">Je založena na přístupové tokeny, které jsou specifické tooa datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e467-106">It is based on access tokens that are specific tooa dataset.</span></span> <span data-ttu-id="3e467-107">Pomocí Azure Active Directory (AAD) jsou vydávány tokeny, na které se používá pro PowerBI.com a vlastní službou jsou vydávány tokeny Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="3e467-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="3e467-108">Když createing zprávu o vložených hello tokeny vydané jsou pro konkrétní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e467-108">When createing an Embedded report, hello tokens issued are for a specific dataset.</span></span> <span data-ttu-id="3e467-109">Tokeny by měly být přidruženy s hello vložte adresu URL na hello stejný element tooensure každý má jedinečný token.</span><span class="sxs-lookup"><span data-stu-id="3e467-109">Tokens should be associated with hello embed URL on hello same element tooensure each has a unique token.</span></span> <span data-ttu-id="3e467-110">V pořadí toocreate zprávu o vložených *Dataset.Read a Workspace.Report.Create* obory musí být zadáno v hello přístupový token.</span><span class="sxs-lookup"><span data-stu-id="3e467-110">In order toocreate an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in hello access token.</span></span>

## <a name="create-access-token-needed-toocreate-new-report"></a><span data-ttu-id="3e467-111">Vytvořit novou sestavu tokenu potřebné toocreate přístup</span><span class="sxs-lookup"><span data-stu-id="3e467-111">Create access token needed toocreate new report</span></span>

<span data-ttu-id="3e467-112">Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON.</span><span class="sxs-lookup"><span data-stu-id="3e467-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="3e467-113">Hello tokeny jsou podepsané hello přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="3e467-113">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="3e467-114">Vložení tokeny, ve výchozím nastavení, jsou použité tooprovide číst přístup jenom k tooembed tooa sestavy do aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e467-114">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="3e467-115">Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.</span><span class="sxs-lookup"><span data-stu-id="3e467-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="3e467-116">Přístupové tokeny musí být vytvořena na serveru hello hello přístupové klíče jsou použité toosign nebo šifrování tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="3e467-116">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="3e467-117">Informace o tom najdete v části toocreate přístupový token, [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="3e467-117">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="3e467-118">Můžete také zkontrolovat hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda.</span><span class="sxs-lookup"><span data-stu-id="3e467-118">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="3e467-119">Tady je příklad co to může vypadat třeba pomocí hello .NET SDK pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="3e467-119">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="3e467-120">V tomto příkladu máme naše id sady dat, který chceme toocreat hello novou sestavu na.</span><span class="sxs-lookup"><span data-stu-id="3e467-120">In this example, we have our dataset id that we want toocreat hello new report on.</span></span> <span data-ttu-id="3e467-121">Potřebujeme tooadd hello obory pro *Dataset.Read a Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="3e467-121">We also need tooadd hello scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="3e467-122">Hello *PowerBIToken třída* vyžaduje instalaci hello [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="3e467-122">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="3e467-123">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="3e467-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="3e467-124">**Kód jazyka C#**</span><span class="sxs-lookup"><span data-stu-id="3e467-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="3e467-125">Vytvoření nové prázdné sestavy</span><span class="sxs-lookup"><span data-stu-id="3e467-125">Create a new blank report</span></span>

<span data-ttu-id="3e467-126">V pořadí toocreate nové sestavy, vytvořte hello konfigurace je nutné zadat.</span><span class="sxs-lookup"><span data-stu-id="3e467-126">In order toocreate a new report, hello create configuration should be provided.</span></span> <span data-ttu-id="3e467-127">To by mělo zahrnovat hello přístupový token, hello embedURL a hello datasetID, které chceme toocreate hello sestavy proti.</span><span class="sxs-lookup"><span data-stu-id="3e467-127">This should include hello access token, hello embedURL and hello datasetID that we want toocreate hello report against.</span></span> <span data-ttu-id="3e467-128">To je nutné nainstalovat hello nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="3e467-128">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="3e467-129">Hello embedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="3e467-129">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="3e467-130">Můžete použít hello [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkce.</span><span class="sxs-lookup"><span data-stu-id="3e467-130">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="3e467-131">Také nabízí příklady kódu pro hello různé operace, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3e467-131">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="3e467-132">**Instalace balíčku NuGet**</span><span class="sxs-lookup"><span data-stu-id="3e467-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="3e467-133">**Kód jazyka JavaScript**</span><span class="sxs-lookup"><span data-stu-id="3e467-133">**JavaScript code**</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

<span data-ttu-id="3e467-134">Volání metody *powerbi.createReport()* bude prázdného plátna v režimu úprav zobrazí v rámci hello *div* elementu.</span><span class="sxs-lookup"><span data-stu-id="3e467-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within hello *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="3e467-135">Uložit nové sestavy</span><span class="sxs-lookup"><span data-stu-id="3e467-135">Save new reports</span></span>

<span data-ttu-id="3e467-136">Sestava Hello ve skutečnosti nevytvoří, dokud volání hello **uložit jako** operaci.</span><span class="sxs-lookup"><span data-stu-id="3e467-136">hello report will not actually be created until you call hello **save as** operation.</span></span> <span data-ttu-id="3e467-137">Tento krok můžete provést z nabídky soubor nebo pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3e467-137">This can be done from file menu or from JavaScript.</span></span>

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> <span data-ttu-id="3e467-138">Vytvoření nové sestavy až po **uložit jako** je volána.</span><span class="sxs-lookup"><span data-stu-id="3e467-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="3e467-139">Po uložení se hello plátno, bude mít hello datovou sadu v sestavě režim a není hello upravit.</span><span class="sxs-lookup"><span data-stu-id="3e467-139">After you save, hello canvas will still show hello dataset in edit mode and not hello report.</span></span> <span data-ttu-id="3e467-140">Budete potřebovat tooreload hello novou sestavu jako jiných sestavách.</span><span class="sxs-lookup"><span data-stu-id="3e467-140">You will need tooreload hello new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a><span data-ttu-id="3e467-141">Nová sestava zatížení hello</span><span class="sxs-lookup"><span data-stu-id="3e467-141">Load hello new report</span></span>

<span data-ttu-id="3e467-142">Pořadí toointeract s novou sestavu hello je nutné ji hello speciálně pro hello nové sestavy musí být vystavené stejným způsobem jako aplikace hello vloží pravidelných sestav, což znamená, nový token, a potom volání hello vložení metoda tooembed.</span><span class="sxs-lookup"><span data-stu-id="3e467-142">In order toointeract with hello new report you need tooembed it in hello same way hello application embeds a regular report, meaning, a new token must be issued specifically for hello new report and then call hello embed method.</span></span>

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a><span data-ttu-id="3e467-143">Automatizovat uložení a načtení nové sestavy pomocí hello "uložené" události</span><span class="sxs-lookup"><span data-stu-id="3e467-143">Automate save and load of a new report using hello "saved" event</span></span>

<span data-ttu-id="3e467-144">V pořadí tooautomate hello proces "Uložit jako" a pak načítání hello novou sestavu můžete provést použití hello "uložené" události.</span><span class="sxs-lookup"><span data-stu-id="3e467-144">In order tooautomate hello process of "save as" and then loading hello new report you can make use of hello "saved" Event.</span></span> <span data-ttu-id="3e467-145">Tato událost je aktivována, po dokončení hello operace uložení a vrátí objekt Json obsahující hello nové reportId, název sestavy, staré reportId hello (pokud existuje) a pokud byla operace hello uložit jako nebo uložit.</span><span class="sxs-lookup"><span data-stu-id="3e467-145">This event is fired when hello save operation is complete and it returns a Json object containing hello new reportId, report name, hello old reportId(if there was one) and if hello operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="3e467-146">tooAutomate hello proces můžete naslouchat na události "uložené" hello, provést nové reportId hello, vytvořit nový token hello a vložit novou sestavu hello s ním.</span><span class="sxs-lookup"><span data-stu-id="3e467-146">tooAutomate hello process you can listen on hello "saved" event, take hello new reportId, create hello new token and embed hello new report with it.</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints tooLog window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that hello application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide hello wanted scopes*/);
        
        
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

## <a name="see-also"></a><span data-ttu-id="3e467-147">Viz také</span><span class="sxs-lookup"><span data-stu-id="3e467-147">See also</span></span>

[<span data-ttu-id="3e467-148">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="3e467-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="3e467-149">Ukládání sestav</span><span class="sxs-lookup"><span data-stu-id="3e467-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="3e467-150">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="3e467-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="3e467-151">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="3e467-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="3e467-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="3e467-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="3e467-153">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="3e467-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="3e467-154">Power BI základní NuGut balíčku</span><span class="sxs-lookup"><span data-stu-id="3e467-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="3e467-155">Power BI JavaScript balíčku</span><span class="sxs-lookup"><span data-stu-id="3e467-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="3e467-156">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="3e467-156">More questions?</span></span> [<span data-ttu-id="3e467-157">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="3e467-157">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
