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
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="f11e2-104">Uložení sestavy v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="f11e2-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="f11e2-105">Zjistěte, jak toosave sestavy v rámci Power BI embedded.</span><span class="sxs-lookup"><span data-stu-id="f11e2-105">Learn how toosave reports within Power BI embedded.</span></span> <span data-ttu-id="f11e2-106">To vyžaduje příslušná oprávnění v pořadí toowork úspěšně.</span><span class="sxs-lookup"><span data-stu-id="f11e2-106">This requires proper permissions in order toowork successfully.</span></span>

<span data-ttu-id="f11e2-107">V rámci Power BI Embedded můžete upravit existující sestavy a uložíte.</span><span class="sxs-lookup"><span data-stu-id="f11e2-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="f11e2-108">Můžete také vytvořit novou sestavu a uložit jako novou sestavu toocreate, jeden.</span><span class="sxs-lookup"><span data-stu-id="f11e2-108">You can also create a new report and save as a new report toocreate one.</span></span>

<span data-ttu-id="f11e2-109">V pořadí toosave sestavy je nutné nejprve toocreate token hello konkrétní sestavy s obory správné hello:</span><span class="sxs-lookup"><span data-stu-id="f11e2-109">In order toosave a report you first need toocreate a token for hello specific report with hello right scopes:</span></span>

* <span data-ttu-id="f11e2-110">je vyžadován tooenable uložit Report.ReadWrite oboru</span><span class="sxs-lookup"><span data-stu-id="f11e2-110">tooenable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="f11e2-111">tooenable uložit jako, Report.Read a Workspace.Report.Copy obory jsou požadovány</span><span class="sxs-lookup"><span data-stu-id="f11e2-111">tooenable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="f11e2-112">Uložit tooenable a uložit jako, Report.ReadWrite a Workspace.Report.Copy jsou vyžadován</span><span class="sxs-lookup"><span data-stu-id="f11e2-112">tooenable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="f11e2-113">V uvedeném pořadí v pořadí tooenable hello správné uložit nebo uložit jako tlačítek v nabídce Soubor můžete potřebovat tooprovide hello pravým oprávnění v konfiguraci vložení hello když jste vložení hello sestavy:</span><span class="sxs-lookup"><span data-stu-id="f11e2-113">Respectively in order tooenable hello right save/save as buttons in file menu you need tooprovide hello right permission in hello Embed configuration when you Embed hello report:</span></span>

* <span data-ttu-id="f11e2-114">modely. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="f11e2-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="f11e2-115">modely. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="f11e2-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="f11e2-116">modely. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="f11e2-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="f11e2-117">Přístupový token musí také odpovídající obory hello.</span><span class="sxs-lookup"><span data-stu-id="f11e2-117">Your access token also needs hello appropriate scopes.</span></span> <span data-ttu-id="f11e2-118">Další informace najdete v tématu [obory](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="f11e2-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="f11e2-119">Vložení sestavy v režimu úprav</span><span class="sxs-lookup"><span data-stu-id="f11e2-119">Embed report in edit mode</span></span>

<span data-ttu-id="f11e2-120">Umožňuje například chcete tooEmbed sestavu v režimu úprav uvnitř vaší aplikace, toodo tak, aby předat vlastnosti oprávnění hello v konfiguraci vložení a volání powerbi.embed().</span><span class="sxs-lookup"><span data-stu-id="f11e2-120">Let's say you want tooEmbed a report in edit mode inside your app, toodo so just pass hello right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="f11e2-121">Budete potřebovat oprávnění toosupply a viewMode v pořadí toosee hello uložit a uložit jako tlačítka v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="f11e2-121">You will need toosupply permissions and a viewMode in order toosee hello save and save as buttons when in edit mode.</span></span> <span data-ttu-id="f11e2-122">Další informace najdete v tématu [vložení podrobnosti konfigurace](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="f11e2-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="f11e2-123">Například v jazyce JavaScript:</span><span class="sxs-lookup"><span data-stu-id="f11e2-123">For example in JavaScript:</span></span>

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

<span data-ttu-id="f11e2-124">Nyní sestavy budou vloženy do vaší aplikace v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="f11e2-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="f11e2-125">Uloží sestavu</span><span class="sxs-lookup"><span data-stu-id="f11e2-125">Save report</span></span>

<span data-ttu-id="f11e2-126">Po upravit Embbeding hello sestavy v režimu se oprávnění a práva tokenu hello hello sestavu můžete uložit z nabídky Soubor hello nebo pomocí jazyka javascript:</span><span class="sxs-lookup"><span data-stu-id="f11e2-126">After Embbeding hello report in edit mode with hello right token and permissions you can save hello report from hello file menu or from javascript:</span></span>

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="f11e2-127">Uložit jako</span><span class="sxs-lookup"><span data-stu-id="f11e2-127">Save as</span></span>

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
> <span data-ttu-id="f11e2-128">Až poté, co *uložit jako* je vytvoření nové sestavy.</span><span class="sxs-lookup"><span data-stu-id="f11e2-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="f11e2-129">Po uložení hello, plátno hello je stále zobrazující hello původní sestavy v upravit režim a není hello novou sestavu.</span><span class="sxs-lookup"><span data-stu-id="f11e2-129">After hello save, hello canvas is still showing hello old report in edit mode and not hello new report.</span></span> <span data-ttu-id="f11e2-130">Budete potřebovat hello tooembed novou se sestavu, která byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f11e2-130">You will need tooembed hello new report that was created.</span></span> <span data-ttu-id="f11e2-131">To vyžaduje nový přístupový token, jako jsou vytvořené na sestavu.</span><span class="sxs-lookup"><span data-stu-id="f11e2-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="f11e2-132">Bude nutné tooload hello novou sestavu po *uložit jako*.</span><span class="sxs-lookup"><span data-stu-id="f11e2-132">You will then need tooload hello new report after a *save as*.</span></span> <span data-ttu-id="f11e2-133">To je podobné tooembedding žádnou sestavu.</span><span class="sxs-lookup"><span data-stu-id="f11e2-133">This is similar tooembedding any report.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="f11e2-134">Viz také</span><span class="sxs-lookup"><span data-stu-id="f11e2-134">See also</span></span>

[<span data-ttu-id="f11e2-135">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="f11e2-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="f11e2-136">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="f11e2-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="f11e2-137">Vytvoření nové sestavy z datové sady</span><span class="sxs-lookup"><span data-stu-id="f11e2-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="f11e2-138">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="f11e2-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="f11e2-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="f11e2-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="f11e2-140">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="f11e2-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="f11e2-141">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="f11e2-141">More questions?</span></span> [<span data-ttu-id="f11e2-142">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="f11e2-142">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

