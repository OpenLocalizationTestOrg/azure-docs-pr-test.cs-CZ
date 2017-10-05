---
title: "Azure Active Directory PoC Playbook Úvod | Microsoft Docs"
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
ms.openlocfilehash: 567f3373594bc53435e8c0bd0a3445dd318af1cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="96c65-104">Ověření služby Azure Active Directory koncept Playbook: Úvod</span><span class="sxs-lookup"><span data-stu-id="96c65-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="96c65-105">Tento článek obsahuje pokyny a prozkoumejte různé Azure AD možnosti testování konceptu (PoC).</span><span class="sxs-lookup"><span data-stu-id="96c65-105">This article provides guidelines to explore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="96c65-106">Předpokládanou cílovou skupinou tohoto dokumentu je Identity architektům, odborníci v oblasti IT a integrátory systémů</span><span class="sxs-lookup"><span data-stu-id="96c65-106">The intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-to-use-this-playbook"></a><span data-ttu-id="96c65-107">Jak používat tento Playbook</span><span class="sxs-lookup"><span data-stu-id="96c65-107">How to use this Playbook</span></span>

1. <span data-ttu-id="96c65-108">Použití [motiv](active-directory-playbook-ingredients.md#theme) části a vyberte vlastnost zájmu na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="96c65-108">Use the [Theme](active-directory-playbook-ingredients.md#theme) section and pick the area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="96c65-109">Rozsah PoC výběrem scénáře, které jsou zarovnané s obchodním cílům.</span><span class="sxs-lookup"><span data-stu-id="96c65-109">Scope the PoC by choosing the scenarios that align with your business goals.</span></span> <span data-ttu-id="96c65-110">Kratší tím lépe.</span><span class="sxs-lookup"><span data-stu-id="96c65-110">The shorter the better.</span></span> <span data-ttu-id="96c65-111">Doporučujeme, abyste ho jako krátký a stručným nejblíže k vyjádření hodnota zúčastněným stranám a současně minimalizujete její složitost si uvědomí, je to.</span><span class="sxs-lookup"><span data-stu-id="96c65-111">We recommend doing it as short and concise as possible to convey the value to the stakeholders while minimizing the complexity to realize it.</span></span>  
3. <span data-ttu-id="96c65-112">Použití [implementace](active-directory-playbook-implementation.md) části, abyste pochopili scénáře a co by se rozumí pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="96c65-112">Use the [Implementation](active-directory-playbook-implementation.md) section to understand the scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="96c65-113">V každém scénáři jsme popisují, jak ho nastavit (takzvaný [stavební bloky](active-directory-playbook-building-blocks.md)) a jak se orientovat scénáře.</span><span class="sxs-lookup"><span data-stu-id="96c65-113">In each scenario, we describe how to set it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how to navigate the scenarios.</span></span> 
4. <span data-ttu-id="96c65-114">Každý stavebním blokem vysvětluje požadavky potřebné, jakož i Přibližná doba na dokončení.</span><span class="sxs-lookup"><span data-stu-id="96c65-114">Each building block explains the pre-requisites needed, as well as an approximate time to complete.</span></span> <span data-ttu-id="96c65-115">To vám může pomoci během procesu plánování.</span><span class="sxs-lookup"><span data-stu-id="96c65-115">This can help you during the planning process.</span></span> 
5. <span data-ttu-id="96c65-116">Podle toho, 1 – 3 výše, zadejte [prostředí](active-directory-playbook-ingredients.md#environment) do kterého chcete provést.</span><span class="sxs-lookup"><span data-stu-id="96c65-116">Based on 1-3 Above, define the [Environment](active-directory-playbook-ingredients.md#environment) in which to execute.</span></span> <span data-ttu-id="96c65-117">Doporučujeme usilovat o provozním prostředí podívat, funkční prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="96c65-117">We encourage to strive for a production environment to get a good feel of the experience for your users.</span></span> 
6. <span data-ttu-id="96c65-118">Pokud s konfliktní požadavky, použijte tato matice užitečné kompromis</span><span class="sxs-lookup"><span data-stu-id="96c65-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="96c65-119">Motiv na střed zobrazení hodnoty</span><span class="sxs-lookup"><span data-stu-id="96c65-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="96c65-120">Plynulost připravit, nastavení a spuštění scénáře</span><span class="sxs-lookup"><span data-stu-id="96c65-120">Smoothness to prepare, to set up, and to execute the scenarios</span></span> 
   * <span data-ttu-id="96c65-121">Minimální doba provádění cílové scénáře</span><span class="sxs-lookup"><span data-stu-id="96c65-121">Minimal time to execute the target scenarios</span></span> 
   * <span data-ttu-id="96c65-122">Jako blízko produkční jako vhodný v rámci vaší omezení</span><span class="sxs-lookup"><span data-stu-id="96c65-122">As close to production as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="96c65-123">V tomto článku zobrazí se některé produkty, které jsou uvedené jako příklady ke zvýšení pohodlí a aplikací konkrétní třetí strany.</span><span class="sxs-lookup"><span data-stu-id="96c65-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="96c65-124">Azure AD podporuje tisíce aplikace v našem [galerii aplikací](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) , které můžete použít podle konkrétních potřeb a prostředí.</span><span class="sxs-lookup"><span data-stu-id="96c65-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]