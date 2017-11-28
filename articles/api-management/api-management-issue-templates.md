---
title: "aaaIssue šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toocustomize hello obsah stránky hello problém v hello portál pro vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e12902e52c164f73902a97f15ea550790dfecf1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="196ee-103">Vystavení šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="196ee-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="196ee-104">Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="196ee-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="196ee-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="196ee-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="196ee-106">Hello šablony v této části Povolit obsah hello toocustomize hello problém stránek v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="196ee-106">hello templates in this section allow you toocustomize hello content of hello Issue pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="196ee-107">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="196ee-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="196ee-108">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci hello, ale jsou toochange subjektu z důvodu vylepšení toocontinuous.</span><span class="sxs-lookup"><span data-stu-id="196ee-108">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="196ee-109">Hello za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře hello přechodem toohello potřeby jednotlivých šablony.</span><span class="sxs-lookup"><span data-stu-id="196ee-109">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="196ee-110">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="196ee-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="196ee-111"><a name="IssueList"></a>Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="196ee-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="196ee-112">Hello **seznam problémů** šablona vám umožní toocustomize hello textu hello problém seznamu stránky v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="196ee-112">hello **Issue list** template allows you toocustomize hello body of hello issue list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="196ee-113">![Vystavení portál pro vývojáře seznamu](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "portál pro vývojáře APIM problém seznamu")</span><span class="sxs-lookup"><span data-stu-id="196ee-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="196ee-114">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="196ee-114">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-md-9">  
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>  
  </div>  
</div>  
<div class="row">  
  <div class="col-md-12">  
    {% if issues.size > 0 %}  
    <ul class="list-unstyled">  
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}  
      {% assign replaceString0 = '{0}' %}  
      {% assign replaceString1 = '{1}' %}  
      {% for issue in issues %}  
      <li>  
        <h3>  
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>  
        </h3>  
        <p>{{issue.description}}</p>  
        <em>  
          {% capture state %}{{issue.issueState}}{% endcapture %}  
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}  
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}  
          {{ str1 | replace : replaceString1, devName }}  
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>  
        </em>  
      </li>  
      {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    {% if canReportIssue %}  
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>  
    {% elsif isAuthenticated %}  
    <hr />  
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>  
    {% else %}  
    <hr />  
    <p>  
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}  
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}  
      {{ signIntext | replace : replaceString0, link }}  
    </p>  
    {% endif %}  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="196ee-115">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="196ee-115">Controls</span></span>  
 <span data-ttu-id="196ee-116">Hello `Issue list` šablona může používat následující hello [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="196ee-116">hello `Issue list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="196ee-117">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="196ee-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="196ee-118">Datový model</span><span class="sxs-lookup"><span data-stu-id="196ee-118">Data model</span></span>  
  
|<span data-ttu-id="196ee-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="196ee-119">Property</span></span>|<span data-ttu-id="196ee-120">Typ</span><span class="sxs-lookup"><span data-stu-id="196ee-120">Type</span></span>|<span data-ttu-id="196ee-121">Popis</span><span class="sxs-lookup"><span data-stu-id="196ee-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="196ee-122">Problémy</span><span class="sxs-lookup"><span data-stu-id="196ee-122">Issues</span></span>|<span data-ttu-id="196ee-123">Kolekce [problém](api-management-template-data-model-reference.md#Issue) entity.</span><span class="sxs-lookup"><span data-stu-id="196ee-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="196ee-124">Hello problémy viditelné toohello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="196ee-124">hello issues visible toohello current user.</span></span>|  
|<span data-ttu-id="196ee-125">Stránkování</span><span class="sxs-lookup"><span data-stu-id="196ee-125">Paging</span></span>|<span data-ttu-id="196ee-126">[Stránkování](api-management-template-data-model-reference.md#Paging) entity.</span><span class="sxs-lookup"><span data-stu-id="196ee-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="196ee-127">informace o stránkování Hello pro kolekci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="196ee-127">hello paging information for hello applications collection.</span></span>|  
|<span data-ttu-id="196ee-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="196ee-128">IsAuthenticated</span></span>|<span data-ttu-id="196ee-129">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="196ee-129">boolean</span></span>|<span data-ttu-id="196ee-130">Jestli hello aktuální uživatel je přihlášený toohello portál pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="196ee-130">Whether hello current user is signed-in toohello developer portal.</span></span>|  
|<span data-ttu-id="196ee-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="196ee-131">CanReportIssues</span></span>|<span data-ttu-id="196ee-132">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="196ee-132">boolean</span></span>|<span data-ttu-id="196ee-133">Tom, zda text hello aktuální uživatel nemá oprávnění toofile problém.</span><span class="sxs-lookup"><span data-stu-id="196ee-133">Whether hello current user has permissions toofile an issue.</span></span>|  
|<span data-ttu-id="196ee-134">Search</span><span class="sxs-lookup"><span data-stu-id="196ee-134">Search</span></span>|<span data-ttu-id="196ee-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="196ee-135">string</span></span>|<span data-ttu-id="196ee-136">Tato vlastnost je zastaralá a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="196ee-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="196ee-137">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="196ee-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how tooconnect my application toohello API",  
            "Description": "I'm having trouble connecting my application toohello backend API.",  
            "SubscriptionDeveloperName": "Clayton",  
            "IssueState": "Proposed",  
            "ReportedOn": "2016-04-04T18:46:35.64",  
            "Comments": null,  
            "Attachments": null,  
            "Services": null  
        }  
    ],  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "IsAuthenticated": true,  
    "CanReportIssue": true,  
    "Search": null  
}  
```

## <a name="next-steps"></a><span data-ttu-id="196ee-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="196ee-138">Next steps</span></span>
<span data-ttu-id="196ee-139">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="196ee-139">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
