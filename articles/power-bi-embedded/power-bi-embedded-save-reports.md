---
title: aaaSave sestav v Azure Power BI Embedded | Microsoft Docs
description: "Zjistěte, jak toosave sestavy v rámci Power BI embedded. To vyžaduje příslušná oprávnění v pořadí toowork úspěšně."
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
ms.openlocfilehash: 984537ce1ce1afc787d6c6c9f61ae8d6226d1171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="save-reports-in-power-bi-embedded"></a>Uložení sestavy v Power BI Embedded

Zjistěte, jak toosave sestavy v rámci Power BI embedded. To vyžaduje příslušná oprávnění v pořadí toowork úspěšně.

V rámci Power BI Embedded můžete upravit existující sestavy a uložíte. Můžete také vytvořit novou sestavu a uložit jako novou sestavu toocreate, jeden.

V pořadí toosave sestavy je nutné nejprve toocreate token hello konkrétní sestavy s obory správné hello:

* je vyžadován tooenable uložit Report.ReadWrite oboru
* tooenable uložit jako, Report.Read a Workspace.Report.Copy obory jsou požadovány
* Uložit tooenable a uložit jako, Report.ReadWrite a Workspace.Report.Copy jsou vyžadován

V uvedeném pořadí v pořadí tooenable hello správné uložit nebo uložit jako tlačítek v nabídce Soubor můžete potřebovat tooprovide hello pravým oprávnění v konfiguraci vložení hello když jste vložení hello sestavy:

* modely. Permissions.ReadWrite
* modely. Permissions.Copy
* modely. Permissions.All

> [!NOTE]
> Přístupový token musí také odpovídající obory hello. Další informace najdete v tématu [obory](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Vložení sestavy v režimu úprav

Umožňuje například chcete tooEmbed sestavu v režimu úprav uvnitř vaší aplikace, toodo tak, aby předat vlastnosti oprávnění hello v konfiguraci vložení a volání powerbi.embed(). Budete potřebovat oprávnění toosupply a viewMode v pořadí toosee hello uložit a uložit jako tlačítka v režimu úprav. Další informace najdete v tématu [vložení podrobnosti konfigurace](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Například v jazyce JavaScript:

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used toodescribe hello what and how tooembed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference toohello embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed hello report and display it within hello div container.
    var report = powerbi.embed(reportContainer, config);
```

Nyní sestavy budou vloženy do vaší aplikace v režimu úprav.

## <a name="save-report"></a>Uloží sestavu

Po upravit Embbeding hello sestavy v režimu se oprávnění a práva tokenu hello hello sestavu můžete uložit z nabídky Soubor hello nebo pomocí jazyka javascript:

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Uložit jako

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
> Až poté, co *uložit jako* je vytvoření nové sestavy. Po uložení hello, plátno hello je stále zobrazující hello původní sestavy v upravit režim a není hello novou sestavu. Budete potřebovat hello tooembed novou se sestavu, která byla vytvořena. To vyžaduje nový přístupový token, jako jsou vytvořené na sestavu.

Bude nutné tooload hello novou sestavu po *uložit jako*. To je podobné tooembedding žádnou sestavu.

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

## <a name="see-also"></a>Viz také

[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Vytvoření nové sestavy z datové sady](power-bi-embedded-create-report-from-dataset.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

