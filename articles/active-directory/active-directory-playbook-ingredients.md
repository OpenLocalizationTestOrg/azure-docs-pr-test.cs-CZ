---
title: "Azure Active Directory PoC Playbook přísady | Microsoft Docs"
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
ms.openlocfilehash: d2a0fe280f143d390f5e4ba40e0ebe92d8a4a837
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="5598c-104">Ověření služby Azure Active Directory koncept Playbook složek</span><span class="sxs-lookup"><span data-stu-id="5598c-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="5598c-105">Motiv</span><span class="sxs-lookup"><span data-stu-id="5598c-105">Theme</span></span>
<span data-ttu-id="5598c-106">Azure AD poskytuje řešení identit a přístupu ve více oblastech v podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="5598c-106">Azure AD provides identity and access solutions across multiple areas in the enterprise.</span></span> <span data-ttu-id="5598c-107">Jsme klasifikovat scénáře v těchto oblastech:</span><span class="sxs-lookup"><span data-stu-id="5598c-107">We classify the scenarios in the following areas:</span></span> 

* [<span data-ttu-id="5598c-108">Velký počet aplikací, jednu identitu</span><span class="sxs-lookup"><span data-stu-id="5598c-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="5598c-109">Zvýšit zabezpečení</span><span class="sxs-lookup"><span data-stu-id="5598c-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="5598c-110">Škálování s Self Service</span><span class="sxs-lookup"><span data-stu-id="5598c-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="5598c-111">Definování motivu na rámce PoC umožňuje zaměřit úsilí chodem s obchodními cíli, což často jsou aktivačních událostí zájem testování konceptu na prvním místě.</span><span class="sxs-lookup"><span data-stu-id="5598c-111">Defining a theme to frame the PoC helps to focus the efforts that resonates with business goals, which oftentimes are the triggers of the interest in a proof of concept in the first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="5598c-112">Prostředí</span><span class="sxs-lookup"><span data-stu-id="5598c-112">Environment</span></span>

<span data-ttu-id="5598c-113">Je důležité určit podrobnosti o prostředí, kde bude poskytovat fázi testování koncepce.</span><span class="sxs-lookup"><span data-stu-id="5598c-113">It is important to determine the details of the environment where you will deliver the PoC.</span></span> <span data-ttu-id="5598c-114">V ideálním případě můžete vytvořit při ji po dokončení fázi testování koncepce.</span><span class="sxs-lookup"><span data-stu-id="5598c-114">Ideally you can build upon it after the PoC is completed.</span></span> <span data-ttu-id="5598c-115">Byste měli najít rovnováhu mezi ho jako skutečné nejblíže a nároky na omezení nebo další důležité informace o cílovém prostředí je zásadní.</span><span class="sxs-lookup"><span data-stu-id="5598c-115">The target environment is crucial and you should find the right balance between making it as real as possible and the overhead of constraints or extra considerations.</span></span> <span data-ttu-id="5598c-116">Typické prostředí pro testovací prostředí jsou:</span><span class="sxs-lookup"><span data-stu-id="5598c-116">The typical environments for PoCs are:</span></span>
* <span data-ttu-id="5598c-117">**Produkční:** scénáře budou provedeny v prostředí za provozu a nasazené cloudové služby společnosti Microsoft (produkční AD, Office 365, Azure AD klienta nebo jednotného přihlašování k řešení).</span><span class="sxs-lookup"><span data-stu-id="5598c-117">**Production:** The scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="5598c-118">**Uživatel přijetí testování uživateli nebo vývojová prostředí:** máte infrastruktury testovací (paralelní AD a potenciálně Azure AD klienta nebo jednotného přihlašování k řešení) s testovací data, která se podobá produkční.</span><span class="sxs-lookup"><span data-stu-id="5598c-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="5598c-119">Testovací prostředí je obvykle sdílet mezi více projektů v podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="5598c-119">Typically, the test environment is shared across multiple projects in the enterprise.</span></span>

<span data-ttu-id="5598c-120">Většina scénářů v této příručce jsou sčítání ve své podstatě.</span><span class="sxs-lookup"><span data-stu-id="5598c-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="5598c-121">V důsledku toho je lze nasadit v provozním klientovi bez ovlivnění uživatelé mimo fázi testování koncepce.</span><span class="sxs-lookup"><span data-stu-id="5598c-121">As a result, they can be deployed in the production tenant without affecting users outside the PoC.</span></span> <span data-ttu-id="5598c-122">V tomto dokumentu jsme se volání na scénáře, které by neměl mít vliv na úrovni klienta.</span><span class="sxs-lookup"><span data-stu-id="5598c-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="5598c-123">V takových případech můžete chtít zvážit mimo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="5598c-123">In those cases, you might want to consider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="5598c-124">Cíloví uživatelé</span><span class="sxs-lookup"><span data-stu-id="5598c-124">Target Users</span></span>

<span data-ttu-id="5598c-125">Je důležité k určení cílové sady uživatelů, kteří budou uplatňovat scénáře, zejména v případě, že prostředí je produkčním i testovacím.</span><span class="sxs-lookup"><span data-stu-id="5598c-125">It is important to determine the target set of users that will exercise the scenarios, especially when the environment is production or test.</span></span> <span data-ttu-id="5598c-126">Kategorie uživatelů cíl pro testování koncepce jsou:</span><span class="sxs-lookup"><span data-stu-id="5598c-126">The categories of target users for PoC are:</span></span>
* <span data-ttu-id="5598c-127">**Pilotní uživatelé:** skutečné uživatelů v rámci prostředí, který bude používat řešení s účtem, používají pro své každodenní úlohy funkce</span><span class="sxs-lookup"><span data-stu-id="5598c-127">**Pilot Users:** Real users in the environment that will be using the solution with the account they use for their day to day job functions</span></span>
* <span data-ttu-id="5598c-128">**Testovací uživatele:** testovací účty, které jsou vytvořeny v prostředí</span><span class="sxs-lookup"><span data-stu-id="5598c-128">**Test Users:** Test accounts created in the environment</span></span> 

<span data-ttu-id="5598c-129">Většina scénářů v této příručce můžete provést podle uživatelů pilotního nasazení.</span><span class="sxs-lookup"><span data-stu-id="5598c-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="5598c-130">V tomto dokumentu jsme se být volání si důležité informace o cílové uživatele v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="5598c-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]