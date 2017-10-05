---
title: "Vystavení šablon ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky problém v portálu pro vývojáře ve službě Azure API Management."
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
ms.openlocfilehash: e13344df198bca4f73c75fa58221436b94e2f258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="2655a-103">Vystavení šablon ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2655a-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="2655a-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="2655a-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="2655a-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="2655a-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="2655a-106">Šablony v této části umožňují přizpůsobit obsah stránky problém v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2655a-106">The templates in this section allow you to customize the content of the Issue pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="2655a-107">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="2655a-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="2655a-108">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci, ale mohou být změněna z důvodu průběžné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="2655a-108">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="2655a-109">Za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře přechodem na jednotlivé požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="2655a-109">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="2655a-110">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2655a-110">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="2655a-111"><a name="IssueList"></a>Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="2655a-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="2655a-112">**Seznam problémů** šablona umožňuje přizpůsobení textu stránky seznamu problém v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2655a-112">The **Issue list** template allows you to customize the body of the issue list page in the developer portal.</span></span>  
  
 <span data-ttu-id="2655a-113">![Vystavení portál pro vývojáře seznamu](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "portál pro vývojáře APIM problém seznamu")</span><span class="sxs-lookup"><span data-stu-id="2655a-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="2655a-114">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="2655a-114">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="2655a-115">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="2655a-115">Controls</span></span>  
 <span data-ttu-id="2655a-116">`Issue list` Šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2655a-116">The `Issue list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="2655a-117">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="2655a-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="2655a-118">Datový model</span><span class="sxs-lookup"><span data-stu-id="2655a-118">Data model</span></span>  
  
|<span data-ttu-id="2655a-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2655a-119">Property</span></span>|<span data-ttu-id="2655a-120">Typ</span><span class="sxs-lookup"><span data-stu-id="2655a-120">Type</span></span>|<span data-ttu-id="2655a-121">Popis</span><span class="sxs-lookup"><span data-stu-id="2655a-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="2655a-122">Problémy</span><span class="sxs-lookup"><span data-stu-id="2655a-122">Issues</span></span>|<span data-ttu-id="2655a-123">Kolekce [problém](api-management-template-data-model-reference.md#Issue) entity.</span><span class="sxs-lookup"><span data-stu-id="2655a-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="2655a-124">Problémy, které jsou viditelné pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="2655a-124">The issues visible to the current user.</span></span>|  
|<span data-ttu-id="2655a-125">Stránkování</span><span class="sxs-lookup"><span data-stu-id="2655a-125">Paging</span></span>|<span data-ttu-id="2655a-126">[Stránkování](api-management-template-data-model-reference.md#Paging) entity.</span><span class="sxs-lookup"><span data-stu-id="2655a-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="2655a-127">Informace o stránkování pro kolekce aplikací.</span><span class="sxs-lookup"><span data-stu-id="2655a-127">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="2655a-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="2655a-128">IsAuthenticated</span></span>|<span data-ttu-id="2655a-129">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="2655a-129">boolean</span></span>|<span data-ttu-id="2655a-130">Jestli má aktuální uživatel je přihlášený k portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="2655a-130">Whether the current user is signed-in to the developer portal.</span></span>|  
|<span data-ttu-id="2655a-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="2655a-131">CanReportIssues</span></span>|<span data-ttu-id="2655a-132">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="2655a-132">boolean</span></span>|<span data-ttu-id="2655a-133">Jestli má aktuální uživatel oprávnění k souboru problém.</span><span class="sxs-lookup"><span data-stu-id="2655a-133">Whether the current user has permissions to file an issue.</span></span>|  
|<span data-ttu-id="2655a-134">Search</span><span class="sxs-lookup"><span data-stu-id="2655a-134">Search</span></span>|<span data-ttu-id="2655a-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="2655a-135">string</span></span>|<span data-ttu-id="2655a-136">Tato vlastnost je zastaralá a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="2655a-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="2655a-137">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="2655a-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how to connect my application to the API",  
            "Description": "I'm having trouble connecting my application to the backend API.",  
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

## <a name="next-steps"></a><span data-ttu-id="2655a-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2655a-138">Next steps</span></span>
<span data-ttu-id="2655a-139">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2655a-139">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>