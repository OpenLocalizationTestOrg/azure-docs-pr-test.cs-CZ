---
title: "Šablony aplikací ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky aplikace v portálu pro vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 6d2d44465800219f16866a621d4822614ac9e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-templates-in-azure-api-management"></a><span data-ttu-id="a25fe-103">Šablony aplikací ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a25fe-103">Application templates in Azure API Management</span></span>
<span data-ttu-id="a25fe-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="a25fe-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="a25fe-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="a25fe-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="a25fe-106">Šablony v této části umožňují přizpůsobit obsah stránky aplikace v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a25fe-106">The templates in this section allow you to customize the content of the Application pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="a25fe-107">Seznam aplikací</span><span class="sxs-lookup"><span data-stu-id="a25fe-107">Application list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="a25fe-108">Aplikace</span><span class="sxs-lookup"><span data-stu-id="a25fe-108">Application</span></span>](#Application)  
  
> [!NOTE]
>  <span data-ttu-id="a25fe-109">Ukázka výchozí šablony jsou zahrnuty v následující dokumentaci, ale mohou být změněna z důvodu průběžné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="a25fe-109">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="a25fe-110">Za provozu výchozí šablony můžete zobrazit v portálu pro vývojáře přechodem na jednotlivé požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="a25fe-110">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="a25fe-111">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="a25fe-111">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="a25fe-112"><a name="ProductList"></a>Seznam aplikací</span><span class="sxs-lookup"><span data-stu-id="a25fe-112"><a name="ProductList"></a> Application list</span></span>  
 <span data-ttu-id="a25fe-113">**Seznam aplikací** šablona umožňuje přizpůsobit text seznamu stránka aplikace v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a25fe-113">The **Application list** template allows you to customize the body of the application list page in the developer portal.</span></span>  
  
 <span data-ttu-id="a25fe-114">![Šablony portálu vývojář stránky v seznamu aplikací](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM aplikace seznamu vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="a25fe-114">![Application List Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Application List Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="a25fe-115">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="a25fe-115">Default template</span></span>  
  
```xml  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "AppStrings|WebApplicationsHeader" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if applications.size > 0 %}  
        <ul class="list-unstyled">  
        {% for app in applications %}  
            <li>  
            {% if app.application.icon.url != "" %}  
                <aside>  
                    <a href="/applications/details/{{app.application.id}}"><img src="{{app.application.icon.url}}" alt="App Icon" /></a>  
                </aside>  
            {% endif %}  
                <h3><a href="/applications/details/{{app.application.id}}">{{app.application.title}}</a></h3>  
                {{app.application.description}}  
            </li>     
        {% endfor %}  
        </ul>  
        <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="a25fe-116">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="a25fe-116">Controls</span></span>  
 <span data-ttu-id="a25fe-117">`Product list` Šablona může používat následující [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="a25fe-117">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="a25fe-118">ovládací prvek stránkování</span><span class="sxs-lookup"><span data-stu-id="a25fe-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="a25fe-119">Datový model</span><span class="sxs-lookup"><span data-stu-id="a25fe-119">Data model</span></span>  
  
|<span data-ttu-id="a25fe-120">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a25fe-120">Property</span></span>|<span data-ttu-id="a25fe-121">Typ</span><span class="sxs-lookup"><span data-stu-id="a25fe-121">Type</span></span>|<span data-ttu-id="a25fe-122">Popis</span><span class="sxs-lookup"><span data-stu-id="a25fe-122">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="a25fe-123">Stránkování</span><span class="sxs-lookup"><span data-stu-id="a25fe-123">Paging</span></span>|<span data-ttu-id="a25fe-124">[Stránkování](api-management-template-data-model-reference.md#Paging) entity.</span><span class="sxs-lookup"><span data-stu-id="a25fe-124">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="a25fe-125">Informace o stránkování pro kolekce aplikací.</span><span class="sxs-lookup"><span data-stu-id="a25fe-125">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="a25fe-126">Aplikace</span><span class="sxs-lookup"><span data-stu-id="a25fe-126">Applications</span></span>|<span data-ttu-id="a25fe-127">Kolekce [aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="a25fe-127">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="a25fe-128">Aplikace, které jsou viditelné pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="a25fe-128">The applications visible to the current user.</span></span>|  
|<span data-ttu-id="a25fe-129">CategoryName</span><span class="sxs-lookup"><span data-stu-id="a25fe-129">CategoryName</span></span>|<span data-ttu-id="a25fe-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a25fe-130">string</span></span>|<span data-ttu-id="a25fe-131">Kategorie aplikace.</span><span class="sxs-lookup"><span data-stu-id="a25fe-131">The category of application.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="a25fe-132">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="a25fe-132">Sample template data</span></span>  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Applications": [  
        {  
            "Application": {  
                "Id": "5702b96fb16653124c8f9ba8",  
                "Title": "Contoso Calculator",  
                "Description": "A simple online calculator.",  
                "Url": null,  
                "Version": null,  
                "Requirements": "Free application with no requirements.",  
                "State": 2,  
                "RegistrationDate": "2016-04-04T18:59:00",  
                "CategoryId": 5,  
                "DeveloperId": "5702b5b0b16653124c8f9ba4",  
                "Attachments": [  
                    {  
                        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                        "Type": "Icon",  
                        "ContentType": "image/png"  
                    },  
                    {  
                        "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
                        "Type": "Screenshot",  
                        "ContentType": "image/png"  
                    }  
                ],  
                "Icon": {  
                    "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                    "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                    "Type": "Icon",  
                    "ContentType": "image/png"  
                }  
            },  
            "CategoryName": "Finance"  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="a25fe-133"><a name="Application"></a>Aplikace</span><span class="sxs-lookup"><span data-stu-id="a25fe-133"><a name="Application"></a> Application</span></span>  
 <span data-ttu-id="a25fe-134">**Aplikace** šablona umožňuje přizpůsobit text stránka aplikace v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="a25fe-134">The **Application** template allows you to customize the body of the application page in the developer portal.</span></span>  
  
 <span data-ttu-id="a25fe-135">![Šablony portálu vývojář stránky aplikací](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM aplikace vývojář stránky portálu šablony")</span><span class="sxs-lookup"><span data-stu-id="a25fe-135">![Application Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Application Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="a25fe-136">Výchozí šablony</span><span class="sxs-lookup"><span data-stu-id="a25fe-136">Default template</span></span>  
  
```xml  
<h2>{{title}}</h2>  
{% if icon.url != "" %}  
<aside class="applications_aside">  
  <div class="image-placeholder">  
    <img src="{{icon.url}}" alt="Application Icon" />  
  </div>  
</aside>  
{% endif %}  
  
<article>  
  {% if url != "" %}  
    <a target="_blank" href="{{url}}">{{url}}</a>  
  {% endif %}  
  
  <p>{{description}}</p>  
  
  {% if requirements != null %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsRequirementsHeader" %}</h3>  
  <p>{{requirements}}</p>  
  {% endif %}  
  
  {% if attachments.size > 0 %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsScreenshotsHeader" %}</h3>  
    {% for screenshot in attachments %}  
      {% if screenshot.type != "Icon" %}  
      <a href="{{screenshot.url}}" data-lightbox="example-set">  
          <img src="/Developer/Applications/Thumbnail?url={{screenshot.url}}" alt='{% localized "AppDetailsStrings|WebApplicationsScreenshotAlt" %}' />  
      </a>  
      {% endif %}  
    {% endfor %}  
  {% endif %}  
</article>  
  
```  
  
### <a name="controls"></a><span data-ttu-id="a25fe-137">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="a25fe-137">Controls</span></span>  
 <span data-ttu-id="a25fe-138">`Application` Šablona neumožňuje použití libovolného [stránka ovládací prvky](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="a25fe-138">The `Application` template does not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="a25fe-139">Datový model</span><span class="sxs-lookup"><span data-stu-id="a25fe-139">Data model</span></span>  
 <span data-ttu-id="a25fe-140">[Aplikace](api-management-template-data-model-reference.md#Application) entity.</span><span class="sxs-lookup"><span data-stu-id="a25fe-140">[Application](api-management-template-data-model-reference.md#Application) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="a25fe-141">Ukázková data šablony</span><span class="sxs-lookup"><span data-stu-id="a25fe-141">Sample template data</span></span>  
  
```json  
{  
    "Id": "5702b96fb16653124c8f9ba8",  
    "Title": "Contoso Calculator",  
    "Description": "A simple online calculator.",  
    "Url": null,  
    "Version": null,  
    "Requirements": "Free application with no requirements.",  
    "State": 2,  
    "RegistrationDate": "2016-04-04T18:59:00",  
    "CategoryId": 5,  
    "DeveloperId": "5702b5b0b16653124c8f9ba4",  
    "Attachments": [  
        {  
            "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
            "Type": "Icon",  
            "ContentType": "image/png"  
        },  
        {  
            "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
            "Type": "Screenshot",  
            "ContentType": "image/png"  
        }  
    ],  
    "Icon": {  
        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
        "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
        "Type": "Icon",  
        "ContentType": "image/png"  
    }  
}  
```

## <a name="next-steps"></a><span data-ttu-id="a25fe-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a25fe-142">Next steps</span></span>
<span data-ttu-id="a25fe-143">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a25fe-143">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>