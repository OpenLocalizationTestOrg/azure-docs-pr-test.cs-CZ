---
title: "aaaAzure Active Directory PoC Playbook přísady | Microsoft Docs"
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
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="6c266-104">Ověření služby Azure Active Directory koncept Playbook složek</span><span class="sxs-lookup"><span data-stu-id="6c266-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="6c266-105">Motiv</span><span class="sxs-lookup"><span data-stu-id="6c266-105">Theme</span></span>
<span data-ttu-id="6c266-106">Azure AD poskytuje řešení identit a přístupu ve více oblastech ve hello podniku.</span><span class="sxs-lookup"><span data-stu-id="6c266-106">Azure AD provides identity and access solutions across multiple areas in hello enterprise.</span></span> <span data-ttu-id="6c266-107">Jsme klasifikovat hello scénáře v hello následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="6c266-107">We classify hello scenarios in hello following areas:</span></span> 

* [<span data-ttu-id="6c266-108">Velký počet aplikací, jednu identitu</span><span class="sxs-lookup"><span data-stu-id="6c266-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="6c266-109">Zvýšit zabezpečení</span><span class="sxs-lookup"><span data-stu-id="6c266-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="6c266-110">Škálování s Self Service</span><span class="sxs-lookup"><span data-stu-id="6c266-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="6c266-111">Definování motiv tooframe hello PoC pomáhá toofocus hello úsilí, chodem s obchodními cíli, což často jsou aktivační události hello hello zájmu v testování konceptu na prvním místě hello.</span><span class="sxs-lookup"><span data-stu-id="6c266-111">Defining a theme tooframe hello PoC helps toofocus hello efforts that resonates with business goals, which oftentimes are hello triggers of hello interest in a proof of concept in hello first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="6c266-112">Prostředí</span><span class="sxs-lookup"><span data-stu-id="6c266-112">Environment</span></span>

<span data-ttu-id="6c266-113">Je důležité toodetermine hello podrobnosti hello prostředí, kde bude poskytovat hello testování koncepce.</span><span class="sxs-lookup"><span data-stu-id="6c266-113">It is important toodetermine hello details of hello environment where you will deliver hello PoC.</span></span> <span data-ttu-id="6c266-114">V ideálním případě můžete vytvořit při ji po dokončení testování koncepce hello.</span><span class="sxs-lookup"><span data-stu-id="6c266-114">Ideally you can build upon it after hello PoC is completed.</span></span> <span data-ttu-id="6c266-115">je velmi důležitý Hello cílové prostředí a byste měli najít rovnováhu mezi hello mezi provedení ho jako skutečné nejblíže hello režii a omezení nebo další aspekty.</span><span class="sxs-lookup"><span data-stu-id="6c266-115">hello target environment is crucial and you should find hello right balance between making it as real as possible and hello overhead of constraints or extra considerations.</span></span> <span data-ttu-id="6c266-116">Hello typické prostředí pro testovací prostředí jsou:</span><span class="sxs-lookup"><span data-stu-id="6c266-116">hello typical environments for PoCs are:</span></span>
* <span data-ttu-id="6c266-117">**Produkční:** hello scénáře budou provedeny v prostředí za provozu a nasazené cloudové služby společnosti Microsoft (produkční AD, Office 365, Azure AD klienta nebo jednotného přihlašování k řešení).</span><span class="sxs-lookup"><span data-stu-id="6c266-117">**Production:** hello scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="6c266-118">**Uživatel přijetí testování uživateli nebo vývojová prostředí:** máte infrastruktury testovací (paralelní AD a potenciálně Azure AD klienta nebo jednotného přihlašování k řešení) s testovací data, která se podobá produkční.</span><span class="sxs-lookup"><span data-stu-id="6c266-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="6c266-119">Obvykle hello testovací prostředí je sdílet mezi více projektů v hello enterprise.</span><span class="sxs-lookup"><span data-stu-id="6c266-119">Typically, hello test environment is shared across multiple projects in hello enterprise.</span></span>

<span data-ttu-id="6c266-120">Většina scénářů v této příručce jsou sčítání ve své podstatě.</span><span class="sxs-lookup"><span data-stu-id="6c266-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="6c266-121">V důsledku toho se dá nasadit v hello produkčního klienta bez ovlivnění uživatelé mimo hello PoC.</span><span class="sxs-lookup"><span data-stu-id="6c266-121">As a result, they can be deployed in hello production tenant without affecting users outside hello PoC.</span></span> <span data-ttu-id="6c266-122">V tomto dokumentu jsme se volání na scénáře, které by neměl mít vliv na úrovni klienta.</span><span class="sxs-lookup"><span data-stu-id="6c266-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="6c266-123">V takových případech můžete chtít tooconsider mimo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c266-123">In those cases, you might want tooconsider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="6c266-124">Cíloví uživatelé</span><span class="sxs-lookup"><span data-stu-id="6c266-124">Target Users</span></span>

<span data-ttu-id="6c266-125">Je důležité toodetermine hello cílovou sadu uživatelů, kteří budou uplatňovat hello scénáře, zejména v případě, že prostředí hello je produkčním i testovacím.</span><span class="sxs-lookup"><span data-stu-id="6c266-125">It is important toodetermine hello target set of users that will exercise hello scenarios, especially when hello environment is production or test.</span></span> <span data-ttu-id="6c266-126">kategorie Hello cíloví uživatelé pro testování koncepce jsou:</span><span class="sxs-lookup"><span data-stu-id="6c266-126">hello categories of target users for PoC are:</span></span>
* <span data-ttu-id="6c266-127">**Pilotní uživatelé:** skutečné uživatelé v hello prostředí, které budou používat hello řešení s účtem hello používají pro své tooday den úlohy funkce</span><span class="sxs-lookup"><span data-stu-id="6c266-127">**Pilot Users:** Real users in hello environment that will be using hello solution with hello account they use for their day tooday job functions</span></span>
* <span data-ttu-id="6c266-128">**Testovací uživatele:** testovací účty, které jsou vytvořené v prostředí hello</span><span class="sxs-lookup"><span data-stu-id="6c266-128">**Test Users:** Test accounts created in hello environment</span></span> 

<span data-ttu-id="6c266-129">Většina scénářů v této příručce můžete provést podle uživatelů pilotního nasazení.</span><span class="sxs-lookup"><span data-stu-id="6c266-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="6c266-130">V tomto dokumentu jsme se být volání si důležité informace o cílové uživatele v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6c266-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]