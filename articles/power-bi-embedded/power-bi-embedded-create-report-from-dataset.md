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
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>Vytvoření nové sestavy z datové sady v Power BI Embedded

Power BI Embedded sestavy lze vytvořit nyní z datové sady ve vaší vlastní aplikaci. 

Metoda ověřování je podobný že sestavy vložit. Je založena na přístupové tokeny, které jsou specifické pro datovou sadu. Pomocí Azure Active Directory (AAD) jsou vydávány tokeny, na které se používá pro PowerBI.com a vlastní službou jsou vydávány tokeny Power BI Embedded.

Když jsou createing vložených sestavy, tokeny vydané pro konkrétní datové sady. Tokeny by měly být přidruženy s adresou URL vložení u stejného elementu k zajistěte, aby byl každý token jedinečný. Chcete-li vytvořit sestavu vložených *Dataset.Read a Workspace.Report.Create* obory musí být zadáno v tokenu přístupu.

## <a name="create-access-token-needed-to-create-new-report"></a>Vytvoření tokenu přístupu, které jsou nutné k vytvoření nové sestavy

Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON. Přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů jsou podepsané tokeny. Vložení tokeny, ve výchozím nastavení, používají se pro čtení pouze přístup k sestavě pro vložení do aplikace. Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.

Přístupové tokeny by se vytvořit na serveru, přístupové klíče se používají pro přihlášení nebo šifrování tokenů. Informace o tom, jak vytvořit token přístupu najdete v tématu [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md). Můžete také zkontrolovat [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda. Tady je příklad co to může vypadat třeba pomocí sady .NET SDK pro Power BI.

V tomto příkladu máme naše id sady dat, který chceme creat – nové sestavy na. Také je potřeba přidat obory *Dataset.Read a Workspace.Report.Create*.

*PowerBIToken třída* je nutné nainstalovat [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**Instalace balíčku NuGet**

```
Install-Package Microsoft.PowerBI.Core
```

**Kód jazyka C#**

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Vytvoření nové prázdné sestavy

Chcete-li vytvořit novou sestavu, je třeba poskytnout vytvořit konfiguraci. To by mělo zahrnovat přístupový token, embedURL a datasetID, který chcete vytvořit sestavu s. To je nutné nainstalovat nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). EmbedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Můžete použít [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) správnou funkci. Také nabízí příklady kódu pro různé operace, které jsou k dispozici.

**Instalace balíčku NuGet**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**Kód jazyka JavaScript**

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

Volání metody *powerbi.createReport()* bude prázdného plátna v režimu úprav zobrazí v rámci *div* elementu.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Uložit nové sestavy

Sestava nebude vytvořena ve skutečnosti, dokud zavoláte **uložit jako** operaci. Tento krok můžete provést z nabídky soubor nebo pomocí jazyka JavaScript.

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
> Vytvoření nové sestavy až po **uložit jako** je volána. Po uložení se na plátno, bude mít datovou sadu v režimu úprav a není sestava. Musíte se znovu načíst nové sestavy, stejně jako u jiných sestavách.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a>Načtení nové sestavy

Aby bylo možné využívat nové sestavy, které potřebujete k vložení stejným způsobem, jakým aplikace vloží regulární sestavy, což znamená, nový token musí být vydaný speciálně pro novou sestavu a pak volat metodu vložení.

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

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>Automatizovat uložení a načtení nové sestavy pomocí "uložené" události

V pořadí pro automatizaci procesu "Uložit jako" a pak načítání nové sestavy, které lze provést použitím "uložené" události. Tato událost je aktivována, pokud uložení bylo dokončeno a vrátí objekt Json obsahující nové reportId, název sestavy, staré reportId (pokud existuje) a pokud byla operace Uložit jako nebo uložit.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

Pro automatizaci procesu můžete naslouchat "uložené" události, proveďte nové reportId, vytvořit nový token a vložit novou sestavu s ním.

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

## <a name="see-also"></a>Viz také

[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Ukládání sestav](power-bi-embedded-save-reports.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI základní NuGut balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)