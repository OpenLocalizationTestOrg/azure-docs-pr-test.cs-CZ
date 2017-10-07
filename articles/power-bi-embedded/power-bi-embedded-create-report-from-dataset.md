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
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>Vytvoření nové sestavy z datové sady v Power BI Embedded

Power BI Embedded sestavy lze vytvořit nyní z datové sady ve vaší vlastní aplikaci. 

Hello metoda ověřování je podobný toothat sestavy pro vložení. Je založena na přístupové tokeny, které jsou specifické tooa datovou sadu. Pomocí Azure Active Directory (AAD) jsou vydávány tokeny, na které se používá pro PowerBI.com a vlastní službou jsou vydávány tokeny Power BI Embedded.

Když createing zprávu o vložených hello tokeny vydané jsou pro konkrétní datové sady. Tokeny by měly být přidruženy s hello vložte adresu URL na hello stejný element tooensure každý má jedinečný token. V pořadí toocreate zprávu o vložených *Dataset.Read a Workspace.Report.Create* obory musí být zadáno v hello přístupový token.

## <a name="create-access-token-needed-toocreate-new-report"></a>Vytvořit novou sestavu tokenu potřebné toocreate přístup

Power BI Embedded používají vložení token, který jsou HMAC podepsané webových tokenů JSON. Hello tokeny jsou podepsané hello přístupový klíč z vaší Azure Power BI Embedded kolekce pracovních prostorů. Vložení tokeny, ve výchozím nastavení, jsou použité tooprovide číst přístup jenom k tooembed tooa sestavy do aplikace. Vložení tokeny jsou vydán pro konkrétní sestavy a by měly být přidružené adrese URL vložení.

Přístupové tokeny musí být vytvořena na serveru hello hello přístupové klíče jsou použité toosign nebo šifrování tokenů hello. Informace o tom najdete v části toocreate přístupový token, [Authenticating a autorizaci s Power BI Embedded](power-bi-embedded-app-token-flow.md). Můžete také zkontrolovat hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoda. Tady je příklad co to může vypadat třeba pomocí hello .NET SDK pro Power BI.

V tomto příkladu máme naše id sady dat, který chceme toocreat hello novou sestavu na. Potřebujeme tooadd hello obory pro *Dataset.Read a Workspace.Report.Create*.

Hello *PowerBIToken třída* vyžaduje instalaci hello [Power BI základní NuGut balíček](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

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

V pořadí toocreate nové sestavy, vytvořte hello konfigurace je nutné zadat. To by mělo zahrnovat hello přístupový token, hello embedURL a hello datasetID, které chceme toocreate hello sestavy proti. To je nutné nainstalovat hello nuget [Power BI JavaScript balíček](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). Hello embedUrl právě bude https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Můžete použít hello [ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkce. Také nabízí příklady kódu pro hello různé operace, které jsou k dispozici.

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
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Volání metody *powerbi.createReport()* bude prázdného plátna v režimu úprav zobrazí v rámci hello *div* elementu.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Uložit nové sestavy

Sestava Hello ve skutečnosti nevytvoří, dokud volání hello **uložit jako** operaci. Tento krok můžete provést z nabídky soubor nebo pomocí jazyka JavaScript.

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
> Vytvoření nové sestavy až po **uložit jako** je volána. Po uložení se hello plátno, bude mít hello datovou sadu v sestavě režim a není hello upravit. Budete potřebovat tooreload hello novou sestavu jako jiných sestavách.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a>Nová sestava zatížení hello

Pořadí toointeract s novou sestavu hello je nutné ji hello speciálně pro hello nové sestavy musí být vystavené stejným způsobem jako aplikace hello vloží pravidelných sestav, což znamená, nový token, a potom volání hello vložení metoda tooembed.

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

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a>Automatizovat uložení a načtení nové sestavy pomocí hello "uložené" události

V pořadí tooautomate hello proces "Uložit jako" a pak načítání hello novou sestavu můžete provést použití hello "uložené" události. Tato událost je aktivována, po dokončení hello operace uložení a vrátí objekt Json obsahující hello nové reportId, název sestavy, staré reportId hello (pokud existuje) a pokud byla operace hello uložit jako nebo uložit.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

tooAutomate hello proces můžete naslouchat na události "uložené" hello, provést nové reportId hello, vytvořit nový token hello a vložit novou sestavu hello s ním.

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

## <a name="see-also"></a>Viz také

[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Ukládání sestav](power-bi-embedded-save-reports.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI základní NuGut balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript balíčku](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)
