---
title: "aaaAzure rozhraní API správy Developer Portal šablony | Microsoft Docs"
description: "Zjistěte, jak toocustomize hello obsah stránky na portálu vývojáře pomocí sadu šablon ve službě Azure API Management."
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
ms.openlocfilehash: 32908712a690dcf530038476ab323978c3e9a1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-developer-portal-templates"></a><span data-ttu-id="efb2e-103">Azure API Management vývojáře portálu šablony</span><span class="sxs-lookup"><span data-stu-id="efb2e-103">Azure API Management Developer Portal Templates</span></span>
<span data-ttu-id="efb2e-104">Azure API Management poskytuje že Hello možnost toocustomize hello obsah stránky na portálu vývojáře pomocí sady šablony, které konfigurace jejich obsahu.</span><span class="sxs-lookup"><span data-stu-id="efb2e-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="efb2e-105">Pomocí [DotLiquid](http://dotliquidmarkup.org/) syntaxe a hello editoru podle své volby, například [DotLiquid pro návrháře](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), a zadané sadu lokalizované [řetězce prostředků](api-management-template-resources.md#strings), [ Prostředky glyfy](api-management-template-resources.md#glyphs), a [stránka ovládací prvky](api-management-page-controls.md), máte flexibilitu tooconfigure hello obsah hello stránek podle potřeby pomocí těchto šablon.</span><span class="sxs-lookup"><span data-stu-id="efb2e-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="efb2e-106">Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="efb2e-106">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  



  
##  <span data-ttu-id="efb2e-107"><a name="DeveloperPortalTemplates"></a>Šablony na portálu vývojáře</span><span class="sxs-lookup"><span data-stu-id="efb2e-107"><a name="DeveloperPortalTemplates"></a> Developer portal templates</span></span>  
  
-   [<span data-ttu-id="efb2e-108">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="efb2e-108">APIs</span></span>](api-management-api-templates.md)  
    -   [<span data-ttu-id="efb2e-109">Rozhraní API seznamu</span><span class="sxs-lookup"><span data-stu-id="efb2e-109">API list</span></span>](api-management-api-templates.md#APIList)  
    -   [<span data-ttu-id="efb2e-110">Operace</span><span class="sxs-lookup"><span data-stu-id="efb2e-110">Operation</span></span>](api-management-api-templates.md#Product)  
    -   [<span data-ttu-id="efb2e-111">Ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="efb2e-111">Code samples</span></span>](api-management-api-templates.md#CodeSamples)  
        -   [<span data-ttu-id="efb2e-112">Curl</span><span class="sxs-lookup"><span data-stu-id="efb2e-112">Curl</span></span>](api-management-api-templates.md#Curl)  
        -   [<span data-ttu-id="efb2e-113">C#</span><span class="sxs-lookup"><span data-stu-id="efb2e-113">C#</span></span>](api-management-api-templates.md#CSharp)  
        -   [<span data-ttu-id="efb2e-114">Java</span><span class="sxs-lookup"><span data-stu-id="efb2e-114">Java</span></span>](api-management-api-templates.md#Stub)  
        -   [<span data-ttu-id="efb2e-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="efb2e-115">JavaScript</span></span>](api-management-api-templates.md#JavaScript)  
        -   [<span data-ttu-id="efb2e-116">Objective C</span><span class="sxs-lookup"><span data-stu-id="efb2e-116">Objective C</span></span>](api-management-api-templates.md#ObjectiveC)  
        -   [<span data-ttu-id="efb2e-117">PHP</span><span class="sxs-lookup"><span data-stu-id="efb2e-117">PHP</span></span>](api-management-api-templates.md#PHP)  
        -   [<span data-ttu-id="efb2e-118">Python</span><span class="sxs-lookup"><span data-stu-id="efb2e-118">Python</span></span>](api-management-api-templates.md#Python)  
        -   [<span data-ttu-id="efb2e-119">Ruby</span><span class="sxs-lookup"><span data-stu-id="efb2e-119">Ruby</span></span>](api-management-api-templates.md#Ruby)  
-   [<span data-ttu-id="efb2e-120">Produkty</span><span class="sxs-lookup"><span data-stu-id="efb2e-120">Products</span></span>](api-management-product-templates.md)  
    -   [<span data-ttu-id="efb2e-121">Seznam produktů</span><span class="sxs-lookup"><span data-stu-id="efb2e-121">Product list</span></span>](api-management-product-templates.md#ProductList)  
    -   [<span data-ttu-id="efb2e-122">Produktu</span><span class="sxs-lookup"><span data-stu-id="efb2e-122">Product</span></span>](api-management-product-templates.md#Product)  
-   [<span data-ttu-id="efb2e-123">Aplikace</span><span class="sxs-lookup"><span data-stu-id="efb2e-123">Applications</span></span>](api-management-application-templates.md)  
    -   [<span data-ttu-id="efb2e-124">Seznam aplikací</span><span class="sxs-lookup"><span data-stu-id="efb2e-124">Application list</span></span>](api-management-application-templates.md#ProductList)  
    -   [<span data-ttu-id="efb2e-125">Aplikace</span><span class="sxs-lookup"><span data-stu-id="efb2e-125">Application</span></span>](api-management-application-templates.md#Application)  
-   [<span data-ttu-id="efb2e-126">Problémy</span><span class="sxs-lookup"><span data-stu-id="efb2e-126">Issues</span></span>](api-management-issue-templates.md)  
    -   [<span data-ttu-id="efb2e-127">Seznam problémů</span><span class="sxs-lookup"><span data-stu-id="efb2e-127">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
-   [<span data-ttu-id="efb2e-128">Profil uživatele</span><span class="sxs-lookup"><span data-stu-id="efb2e-128">User Profile</span></span>](api-management-user-profile-templates.md)  
    -   [<span data-ttu-id="efb2e-129">Profil</span><span class="sxs-lookup"><span data-stu-id="efb2e-129">Profile</span></span>](api-management-user-profile-templates.md#Profile)  
    -   [<span data-ttu-id="efb2e-130">Předplatná</span><span class="sxs-lookup"><span data-stu-id="efb2e-130">Subscriptions</span></span>](api-management-user-profile-templates.md#Subscriptions)  
    -   [<span data-ttu-id="efb2e-131">Aplikace</span><span class="sxs-lookup"><span data-stu-id="efb2e-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
    -   [<span data-ttu-id="efb2e-132">Aktualizovat informace o účtu</span><span class="sxs-lookup"><span data-stu-id="efb2e-132">Update account info</span></span>](api-management-user-profile-templates.md#UpdateAccountInfo)  
-   [<span data-ttu-id="efb2e-133">Stránky</span><span class="sxs-lookup"><span data-stu-id="efb2e-133">Pages</span></span>](api-management-page-templates.md)  
    -   [<span data-ttu-id="efb2e-134">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="efb2e-134">Sign in</span></span>](api-management-page-templates.md#SignIn)  
    -   [<span data-ttu-id="efb2e-135">Registrace</span><span class="sxs-lookup"><span data-stu-id="efb2e-135">Sign up</span></span>](api-management-page-templates.md#SignUp)  
    -   [<span data-ttu-id="efb2e-136">Stránka nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="efb2e-136">Page not found</span></span>](api-management-page-templates.md#PageNotFound)


## <a name="next-steps"></a><span data-ttu-id="efb2e-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="efb2e-137">Next steps</span></span>  
-   [<span data-ttu-id="efb2e-138">Odkaz na šablonu</span><span class="sxs-lookup"><span data-stu-id="efb2e-138">Template reference</span></span>](api-management-developer-portal-templates-reference.md)  
-   [<span data-ttu-id="efb2e-139">Referenční informace k datovému modelu</span><span class="sxs-lookup"><span data-stu-id="efb2e-139">Data model reference</span></span>](api-management-template-data-model-reference.md)  
-   [<span data-ttu-id="efb2e-140">Ovládací prvky stránek</span><span class="sxs-lookup"><span data-stu-id="efb2e-140">Page controls</span></span>](api-management-page-controls.md)  
-   [<span data-ttu-id="efb2e-141">Prostředky šablon</span><span class="sxs-lookup"><span data-stu-id="efb2e-141">Template resources</span></span>](api-management-template-resources.md)