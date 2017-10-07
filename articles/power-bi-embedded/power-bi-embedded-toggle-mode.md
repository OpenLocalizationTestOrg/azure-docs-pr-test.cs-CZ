---
title: "aaaToggle režim prohlížení a úpravy sestavy v Azure Power BI Embedded | Microsoft Docs"
description: "Zjistěte, jak tootoggle režim prohlížení a úpravy sestavy v rámci Power BI Embedded."
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
ms.openlocfilehash: c9e3da5f9ae74d221af650adebde7c9d83b38a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a><span data-ttu-id="873bf-103">Přepínání mezi zobrazení a úprava režimu pro sestavy v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="873bf-103">Toggle between view and edit mode for reports in Power BI Embedded</span></span>

<span data-ttu-id="873bf-104">Zjistěte, jak tootoggle režim prohlížení a úpravy sestavy v rámci Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="873bf-104">Learn how tootoggle between view and edit mode for your reports within Power BI Embedded.</span></span>

## <a name="creating-an-access-token"></a><span data-ttu-id="873bf-105">Vytváření token přístupu</span><span class="sxs-lookup"><span data-stu-id="873bf-105">Creating an access token</span></span>

<span data-ttu-id="873bf-106">Budete potřebovat toocreate přístupový token, který vám dává hello možnost tooboth prohlížení a úpravy sestavy.</span><span class="sxs-lookup"><span data-stu-id="873bf-106">You will need toocreate an access token that gives you hello ability tooboth view and edit a report.</span></span> <span data-ttu-id="873bf-107">tooedit a uložení sestavy, je nutné hello **Report.ReadWrite** token oprávnění.</span><span class="sxs-lookup"><span data-stu-id="873bf-107">tooedit and save a report, you will need hello **Report.ReadWrite** token permission.</span></span> <span data-ttu-id="873bf-108">Další informace najdete v tématu [Authenticating a autorizaci v Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="873bf-108">For more information, see [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

> [!NOTE]
> <span data-ttu-id="873bf-109">To umožní tooedit a uložit změny tooan existující sestavy.</span><span class="sxs-lookup"><span data-stu-id="873bf-109">This will allow you tooedit and save changes tooan existing report.</span></span> <span data-ttu-id="873bf-110">Pokud také chcete hello funkce podporu **uložit jako**, budete potřebovat toosupply další oprávnění.</span><span class="sxs-lookup"><span data-stu-id="873bf-110">If you would also like hello function of supporting **Save As**, you will need toosupply additional permissions.</span></span> <span data-ttu-id="873bf-111">Další informace najdete v tématu [obory](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="873bf-111">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a><span data-ttu-id="873bf-112">Vložení konfigurace</span><span class="sxs-lookup"><span data-stu-id="873bf-112">Embed configuration</span></span>

<span data-ttu-id="873bf-113">Budete potřebovat oprávnění toosupply a viewMode v pořadí toosee hello uložit tlačítko v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="873bf-113">You will need toosupply permissions and a viewMode in order toosee hello save button when in edit mode.</span></span> <span data-ttu-id="873bf-114">Další informace najdete v tématu [vložení podrobnosti konfigurace](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="873bf-114">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="873bf-115">Například v jazyce JavaScript:</span><span class="sxs-lookup"><span data-stu-id="873bf-115">For example in JavaScript:</span></span>

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
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
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

<span data-ttu-id="873bf-116">To bude znamenat tooembed hello sestavu v režimu zobrazení na základě **viewMode** nastaveno příliš**modelů. ViewMode.View**.</span><span class="sxs-lookup"><span data-stu-id="873bf-116">This will indicate tooembed hello report in view mode based on **viewMode** being set too**models.ViewMode.View**.</span></span>

## <a name="view-mode"></a><span data-ttu-id="873bf-117">Režim zobrazení</span><span class="sxs-lookup"><span data-stu-id="873bf-117">View mode</span></span>

<span data-ttu-id="873bf-118">Hello následující JavaScript tooswitch do režimu zobrazení, můžete použít, pokud jste v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="873bf-118">You can use hello following JavaScript tooswitch into view mode, if you are in edit mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooview mode.
report.switchMode("view");

```

## <a name="edit-mode"></a><span data-ttu-id="873bf-119">Režim úprav</span><span class="sxs-lookup"><span data-stu-id="873bf-119">Edit mode</span></span>

<span data-ttu-id="873bf-120">Hello následující JavaScript tooswitch do režimu úprav, můžete použít, pokud jste v zobrazení režimu.</span><span class="sxs-lookup"><span data-stu-id="873bf-120">You can use hello following JavaScript tooswitch into edit mode, if you are in view mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooedit mode.
report.switchMode("edit");

```

## <a name="see-also"></a><span data-ttu-id="873bf-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="873bf-121">See also</span></span>

[<span data-ttu-id="873bf-122">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="873bf-122">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="873bf-123">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="873bf-123">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="873bf-124">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="873bf-124">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="873bf-125">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="873bf-125">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="873bf-126">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="873bf-126">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="873bf-127">Úložiště Git PowerBI CSharp</span><span class="sxs-lookup"><span data-stu-id="873bf-127">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="873bf-128">Úložiště Git PowerBI uzlu</span><span class="sxs-lookup"><span data-stu-id="873bf-128">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="873bf-129">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="873bf-129">More questions?</span></span> [<span data-ttu-id="873bf-130">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="873bf-130">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
