---
title: "Uložit sestav v Azure Power BI Embedded | Microsoft Docs"
description: "Informace o ukládání sestavy v rámci Power BI embedded. To vyžaduje příslušná oprávnění, aby bylo možné správně fungovat."
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
ms.openlocfilehash: ad895004cc2972f2ded81566186325a16d401151
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="36a3d-104">Uložení sestavy v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="36a3d-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="36a3d-105">Informace o ukládání sestavy v rámci Power BI embedded.</span><span class="sxs-lookup"><span data-stu-id="36a3d-105">Learn how to save reports within Power BI embedded.</span></span> <span data-ttu-id="36a3d-106">To vyžaduje příslušná oprávnění, aby bylo možné správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="36a3d-106">This requires proper permissions in order to work successfully.</span></span>

<span data-ttu-id="36a3d-107">V rámci Power BI Embedded můžete upravit existující sestavy a uložíte.</span><span class="sxs-lookup"><span data-stu-id="36a3d-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="36a3d-108">Můžete také vytvořit novou sestavu a uložit jako novou sestavu k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="36a3d-108">You can also create a new report and save as a new report to create one.</span></span>

<span data-ttu-id="36a3d-109">Chcete-li uložit sestavu musíte nejprve vytvořit token pro konkrétní sestavu s správné rozsahy:</span><span class="sxs-lookup"><span data-stu-id="36a3d-109">In order to save a report you first need to create a token for the specific report with the right scopes:</span></span>

* <span data-ttu-id="36a3d-110">Chcete-li povolit ukládání Report.ReadWrite obor je požadován</span><span class="sxs-lookup"><span data-stu-id="36a3d-110">To enable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="36a3d-111">Chcete-li uložit jako, jsou požadovány Report.Read a Workspace.Report.Copy oborů</span><span class="sxs-lookup"><span data-stu-id="36a3d-111">To enable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="36a3d-112">Chcete-li uložit a uložit jako, Report.ReadWrite a Workspace.Report.Copy jsou vyžadován</span><span class="sxs-lookup"><span data-stu-id="36a3d-112">To enable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="36a3d-113">V uvedeném pořadí, aby bylo možné povolit právo uložit nebo uložit jako tlačítka v nabídce soubor potřebujete poskytovat správné oprávnění v konfiguraci vložení při vložení sestavy:</span><span class="sxs-lookup"><span data-stu-id="36a3d-113">Respectively in order to enable the right save/save as buttons in file menu you need to provide the right permission in the Embed configuration when you Embed the report:</span></span>

* <span data-ttu-id="36a3d-114">modely. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="36a3d-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="36a3d-115">modely. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="36a3d-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="36a3d-116">modely. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="36a3d-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="36a3d-117">Přístupový token musí také odpovídající obory.</span><span class="sxs-lookup"><span data-stu-id="36a3d-117">Your access token also needs the appropriate scopes.</span></span> <span data-ttu-id="36a3d-118">Další informace najdete v tématu [obory](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="36a3d-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="36a3d-119">Vložení sestavy v režimu úprav</span><span class="sxs-lookup"><span data-stu-id="36a3d-119">Embed report in edit mode</span></span>

<span data-ttu-id="36a3d-120">Řekněme, že chcete v režimu úprav uvnitř vaší aplikace, Uděláte to tak, aby předat vlastnosti oprávnění v konfiguraci vložení a volání powerbi.embed() vložení sestavy.</span><span class="sxs-lookup"><span data-stu-id="36a3d-120">Let's say you want to Embed a report in edit mode inside your app, to do so just pass the right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="36a3d-121">Budete muset zadat oprávnění a viewMode, chcete-li zobrazit uložení a uložit jako tlačítka, když v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="36a3d-121">You will need to supply permissions and a viewMode in order to see the save and save as buttons when in edit mode.</span></span> <span data-ttu-id="36a3d-122">Další informace najdete v tématu [vložení podrobnosti konfigurace](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="36a3d-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="36a3d-123">Například v jazyce JavaScript:</span><span class="sxs-lookup"><span data-stu-id="36a3d-123">For example in JavaScript:</span></span>

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
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

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

<span data-ttu-id="36a3d-124">Nyní sestavy budou vloženy do vaší aplikace v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="36a3d-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="36a3d-125">Uloží sestavu</span><span class="sxs-lookup"><span data-stu-id="36a3d-125">Save report</span></span>

<span data-ttu-id="36a3d-126">Po Embbeding upravit sestavu v režimu se správné token a oprávnění můžete uložit sestavu v nabídce Soubor nebo z javascript:</span><span class="sxs-lookup"><span data-stu-id="36a3d-126">After Embbeding the report in edit mode with the right token and permissions you can save the report from the file menu or from javascript:</span></span>

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="36a3d-127">Uložit jako</span><span class="sxs-lookup"><span data-stu-id="36a3d-127">Save as</span></span>

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
> <span data-ttu-id="36a3d-128">Až poté, co *uložit jako* je vytvoření nové sestavy.</span><span class="sxs-lookup"><span data-stu-id="36a3d-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="36a3d-129">Po uložení se na plátno stále zobrazuje staré sestavu v režimu úprav a není novou sestavu.</span><span class="sxs-lookup"><span data-stu-id="36a3d-129">After the save, the canvas is still showing the old report in edit mode and not the new report.</span></span> <span data-ttu-id="36a3d-130">Musíte vložit novou sestavu, která byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="36a3d-130">You will need to embed the new report that was created.</span></span> <span data-ttu-id="36a3d-131">To vyžaduje nový přístupový token, jako jsou vytvořené na sestavu.</span><span class="sxs-lookup"><span data-stu-id="36a3d-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="36a3d-132">Pak bude nutné načíst novou sestavu po *uložit jako*.</span><span class="sxs-lookup"><span data-stu-id="36a3d-132">You will then need to load the new report after a *save as*.</span></span> <span data-ttu-id="36a3d-133">To se podobá vkládání žádnou sestavu.</span><span class="sxs-lookup"><span data-stu-id="36a3d-133">This is similar to embedding any report.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="36a3d-134">Viz také</span><span class="sxs-lookup"><span data-stu-id="36a3d-134">See also</span></span>

[<span data-ttu-id="36a3d-135">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="36a3d-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="36a3d-136">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="36a3d-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="36a3d-137">Vytvoření nové sestavy z datové sady</span><span class="sxs-lookup"><span data-stu-id="36a3d-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="36a3d-138">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="36a3d-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="36a3d-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="36a3d-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="36a3d-140">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="36a3d-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="36a3d-141">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="36a3d-141">More questions?</span></span> [<span data-ttu-id="36a3d-142">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="36a3d-142">Try the Power BI Community</span></span>](http://community.powerbi.com/)

