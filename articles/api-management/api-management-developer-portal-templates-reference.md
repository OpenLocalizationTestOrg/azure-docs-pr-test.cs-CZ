---
title: "Azure API Management vývojáře portálu šablony | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit obsah stránky na portálu vývojáře pomocí sadu šablon ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 5189f3d8-2a4c-4dc8-ab19-11c7df0114d4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: dc3f32a3cff2e66a798bd8f6c19c6b56a47643ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-developer-portal-templates"></a><span data-ttu-id="c4ff2-103">Azure API Management vývojáře portálu šablony</span><span class="sxs-lookup"><span data-stu-id="c4ff2-103">Azure API Management Developer Portal Templates</span></span>
<span data-ttu-id="c4ff2-104">Azure API Management poskytuje schopnost přizpůsobit obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="c4ff2-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="c4ff2-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a lokalizované zadaný sadu [řetězce prostředků](api-management-template-resources.md#strings), [glyfy prostředky](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu při konfiguraci obsahu stránek, podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="c4ff2-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="c4ff2-106">Další informace o práci se šablonami najdete v tématu [postup přizpůsobení portálu pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c4ff2-106">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  



  
##  <span data-ttu-id="c4ff2-107"><a name="DeveloperPortalTemplates"></a>Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="c4ff2-107"><a name="DeveloperPortalTemplates"></a> Developer portal templates</span></span>  
  
-   [<span data-ttu-id="c4ff2-108">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c4ff2-108">APIs</span></span>](api-management-api-templates.md)  
    -   [<span data-ttu-id="c4ff2-109">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-109">API list</span></span>](api-management-api-templates.md#APIList)  
    -   [<span data-ttu-id="c4ff2-110">Operace</span><span class="sxs-lookup"><span data-stu-id="c4ff2-110">Operation</span></span>](api-management-api-templates.md#Product)  
    -   [<span data-ttu-id="c4ff2-111">Ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-111">Code samples</span></span>](api-management-api-templates.md#CodeSamples)  
        -   [<span data-ttu-id="c4ff2-112">Curl</span><span class="sxs-lookup"><span data-stu-id="c4ff2-112">Curl</span></span>](api-management-api-templates.md#Curl)  
        -   [<span data-ttu-id="c4ff2-113">C#</span><span class="sxs-lookup"><span data-stu-id="c4ff2-113">C#</span></span>](api-management-api-templates.md#CSharp)  
        -   [<span data-ttu-id="c4ff2-114">Java</span><span class="sxs-lookup"><span data-stu-id="c4ff2-114">Java</span></span>](api-management-api-templates.md#Stub)  
        -   [<span data-ttu-id="c4ff2-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="c4ff2-115">JavaScript</span></span>](api-management-api-templates.md#JavaScript)  
        -   [<span data-ttu-id="c4ff2-116">Objective C</span><span class="sxs-lookup"><span data-stu-id="c4ff2-116">Objective C</span></span>](api-management-api-templates.md#ObjectiveC)  
        -   [<span data-ttu-id="c4ff2-117">PHP</span><span class="sxs-lookup"><span data-stu-id="c4ff2-117">PHP</span></span>](api-management-api-templates.md#PHP)  
        -   [<span data-ttu-id="c4ff2-118">Python</span><span class="sxs-lookup"><span data-stu-id="c4ff2-118">Python</span></span>](api-management-api-templates.md#Python)  
        -   [<span data-ttu-id="c4ff2-119">Ruby</span><span class="sxs-lookup"><span data-stu-id="c4ff2-119">Ruby</span></span>](api-management-api-templates.md#Ruby)  
-   [<span data-ttu-id="c4ff2-120">Produkty</span><span class="sxs-lookup"><span data-stu-id="c4ff2-120">Products</span></span>](api-management-product-templates.md)  
    -   [<span data-ttu-id="c4ff2-121">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="c4ff2-121">Product list</span></span>](api-management-product-templates.md#ProductList)  
    -   [<span data-ttu-id="c4ff2-122">Produktu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-122">Product</span></span>](api-management-product-templates.md#Product)  
-   [<span data-ttu-id="c4ff2-123">Aplikace</span><span class="sxs-lookup"><span data-stu-id="c4ff2-123">Applications</span></span>](api-management-application-templates.md)  
    -   [<span data-ttu-id="c4ff2-124">Seznam aplikací</span><span class="sxs-lookup"><span data-stu-id="c4ff2-124">Application list</span></span>](api-management-application-templates.md#ProductList)  
    -   [<span data-ttu-id="c4ff2-125">Aplikace</span><span class="sxs-lookup"><span data-stu-id="c4ff2-125">Application</span></span>](api-management-application-templates.md#Application)  
-   [<span data-ttu-id="c4ff2-126">Problémy</span><span class="sxs-lookup"><span data-stu-id="c4ff2-126">Issues</span></span>](api-management-issue-templates.md)  
    -   [<span data-ttu-id="c4ff2-127">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="c4ff2-127">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
-   [<span data-ttu-id="c4ff2-128">Profil uživatele</span><span class="sxs-lookup"><span data-stu-id="c4ff2-128">User Profile</span></span>](api-management-user-profile-templates.md)  
    -   [<span data-ttu-id="c4ff2-129">Profil</span><span class="sxs-lookup"><span data-stu-id="c4ff2-129">Profile</span></span>](api-management-user-profile-templates.md#Profile)  
    -   [<span data-ttu-id="c4ff2-130">Předplatná</span><span class="sxs-lookup"><span data-stu-id="c4ff2-130">Subscriptions</span></span>](api-management-user-profile-templates.md#Subscriptions)  
    -   [<span data-ttu-id="c4ff2-131">Aplikace</span><span class="sxs-lookup"><span data-stu-id="c4ff2-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
    -   [<span data-ttu-id="c4ff2-132">Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-132">Update account info</span></span>](api-management-user-profile-templates.md#UpdateAccountInfo)  
-   [<span data-ttu-id="c4ff2-133">Stránky</span><span class="sxs-lookup"><span data-stu-id="c4ff2-133">Pages</span></span>](api-management-page-templates.md)  
    -   [<span data-ttu-id="c4ff2-134">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="c4ff2-134">Sign in</span></span>](api-management-page-templates.md#SignIn)  
    -   [<span data-ttu-id="c4ff2-135">Registrace</span><span class="sxs-lookup"><span data-stu-id="c4ff2-135">Sign up</span></span>](api-management-page-templates.md#SignUp)  
    -   [<span data-ttu-id="c4ff2-136">Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="c4ff2-136">Page not found</span></span>](api-management-page-templates.md#PageNotFound)


## <a name="next-steps"></a><span data-ttu-id="c4ff2-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4ff2-137">Next steps</span></span>  
-   [<span data-ttu-id="c4ff2-138">Odkaz na šablonu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-138">Template reference</span></span>](api-management-developer-portal-templates-reference.md)  
-   [<span data-ttu-id="c4ff2-139">Referenční informace k datovému modelu</span><span class="sxs-lookup"><span data-stu-id="c4ff2-139">Data model reference</span></span>](api-management-template-data-model-reference.md)  
-   [<span data-ttu-id="c4ff2-140">Ovládací prvky stránek</span><span class="sxs-lookup"><span data-stu-id="c4ff2-140">Page controls</span></span>](api-management-page-controls.md)  
-   [<span data-ttu-id="c4ff2-141">Prostředky šablon</span><span class="sxs-lookup"><span data-stu-id="c4ff2-141">Template resources</span></span>](api-management-template-resources.md)