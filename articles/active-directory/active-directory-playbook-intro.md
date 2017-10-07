---
title: "aaaAzure Active Directory PoC Playbook Úvod | Microsoft Docs"
description: "Prozkoumejte a rychle implementovat scénáře identita a správa přístupu"
services: active-directory
keywords: "Azure active directory, scénářem, testování konceptu, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="2140b-104">Ověření služby Azure Active Directory koncept Playbook: Úvod</span><span class="sxs-lookup"><span data-stu-id="2140b-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="2140b-105">Tento článek obsahuje pokyny pro možnosti tooexplore různých Azure AD testování konceptu (PoC).</span><span class="sxs-lookup"><span data-stu-id="2140b-105">This article provides guidelines tooexplore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="2140b-106">Hello zamýšlená cílovou skupinu tohoto dokumentu je Identity architektům, odborníci v oblasti IT a integrátory systémů</span><span class="sxs-lookup"><span data-stu-id="2140b-106">hello intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-toouse-this-playbook"></a><span data-ttu-id="2140b-107">Jak toouse tento Playbook</span><span class="sxs-lookup"><span data-stu-id="2140b-107">How toouse this Playbook</span></span>

1. <span data-ttu-id="2140b-108">Použití hello [motiv](active-directory-playbook-ingredients.md#theme) části a vyberte vlastnost hello zájmu na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="2140b-108">Use hello [Theme](active-directory-playbook-ingredients.md#theme) section and pick hello area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="2140b-109">Obor hello PoC výběrem hello scénáře, které jsou zarovnané s obchodním cílům.</span><span class="sxs-lookup"><span data-stu-id="2140b-109">Scope hello PoC by choosing hello scenarios that align with your business goals.</span></span> <span data-ttu-id="2140b-110">Hello kratší hello lepší.</span><span class="sxs-lookup"><span data-stu-id="2140b-110">hello shorter hello better.</span></span> <span data-ttu-id="2140b-111">Doporučujeme, abyste to jako krátké a stručným jako hodnota hello možné tooconvey toohello zúčastněným stranám a současně minimalizujete její hello složitost toorealize ho.</span><span class="sxs-lookup"><span data-stu-id="2140b-111">We recommend doing it as short and concise as possible tooconvey hello value toohello stakeholders while minimizing hello complexity toorealize it.</span></span>  
3. <span data-ttu-id="2140b-112">Použití hello [implementace](active-directory-playbook-implementation.md) části toounderstand hello scénáře a by jejich význam pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="2140b-112">Use hello [Implementation](active-directory-playbook-implementation.md) section toounderstand hello scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="2140b-113">V každém scénáři jsme popisují, jak tooset ho nahoru (takzvaný [stavební bloky](active-directory-playbook-building-blocks.md)), a jak toonavigate hello scénáře.</span><span class="sxs-lookup"><span data-stu-id="2140b-113">In each scenario, we describe how tooset it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how toonavigate hello scenarios.</span></span> 
4. <span data-ttu-id="2140b-114">Každý stavebním blokem vysvětluje nezbytných požadavků hello, jakož i toocomplete Přibližná doba.</span><span class="sxs-lookup"><span data-stu-id="2140b-114">Each building block explains hello pre-requisites needed, as well as an approximate time toocomplete.</span></span> <span data-ttu-id="2140b-115">To vám může pomoci při plánování procesu hello.</span><span class="sxs-lookup"><span data-stu-id="2140b-115">This can help you during hello planning process.</span></span> 
5. <span data-ttu-id="2140b-116">Podle toho, 1 – 3 výše, definovat hello [prostředí](active-directory-playbook-ingredients.md#environment) v které tooexecute.</span><span class="sxs-lookup"><span data-stu-id="2140b-116">Based on 1-3 Above, define hello [Environment](active-directory-playbook-ingredients.md#environment) in which tooexecute.</span></span> <span data-ttu-id="2140b-117">Doporučujeme toostrive pro produkční prostředí tooget dobrý chování hello prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="2140b-117">We encourage toostrive for a production environment tooget a good feel of hello experience for your users.</span></span> 
6. <span data-ttu-id="2140b-118">Pokud s konfliktní požadavky, použijte tato matice užitečné kompromis</span><span class="sxs-lookup"><span data-stu-id="2140b-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="2140b-119">Motiv na střed zobrazení hodnoty</span><span class="sxs-lookup"><span data-stu-id="2140b-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="2140b-120">Scénáře plynulost tooprepare, tooset nahoru a tooexecute hello</span><span class="sxs-lookup"><span data-stu-id="2140b-120">Smoothness tooprepare, tooset up, and tooexecute hello scenarios</span></span> 
   * <span data-ttu-id="2140b-121">Minimální doba tooexecute hello cílové scénáře</span><span class="sxs-lookup"><span data-stu-id="2140b-121">Minimal time tooexecute hello target scenarios</span></span> 
   * <span data-ttu-id="2140b-122">Jako zavřít tooproduction jako vhodný v rámci vaší omezení</span><span class="sxs-lookup"><span data-stu-id="2140b-122">As close tooproduction as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="2140b-123">V tomto článku zobrazí se některé produkty, které jsou uvedené jako příklady ke zvýšení pohodlí a aplikací konkrétní třetí strany.</span><span class="sxs-lookup"><span data-stu-id="2140b-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="2140b-124">Azure AD podporuje tisíce aplikace v našem [galerii aplikací](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) , které můžete použít podle konkrétních potřeb a prostředí.</span><span class="sxs-lookup"><span data-stu-id="2140b-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]