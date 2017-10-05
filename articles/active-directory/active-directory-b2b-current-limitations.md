---
title: "Omezení spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Aktuální omezení pro spolupráci Azure Active Directory s B2B"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 581e5d1fb5fb08d0dc89ed2c85edcb5f0005650b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="ed35c-103">Omezení spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="ed35c-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="ed35c-104">Spolupráce Azure Active Directory (Azure AD) s B2B aktuálně podléhá omezení popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ed35c-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject to the limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="ed35c-105">Možné double aplikace Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="ed35c-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="ed35c-106">S B2B Azure AD můžete vynutit ověřování Multi-Factor authentication v organizaci prostředků (pozváním organizace).</span><span class="sxs-lookup"><span data-stu-id="ed35c-106">With Azure AD B2B, you can enforce multi-factor authentication at the resource organization (the inviting organization).</span></span> <span data-ttu-id="ed35c-107">Důvody tohoto přístupu jsou podrobně popsané na [podmíněného přístupu pro uživatele spolupráce B2B](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="ed35c-107">The reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="ed35c-108">Pokud partnera již obsahuje službu Multi-Factor authentication nastavit a vynutit, uživatelé pravděpodobně k provedení ověření jednou v jejich domovskou organizaci a potom znovu za vaše.</span><span class="sxs-lookup"><span data-stu-id="ed35c-108">If a partner already has multi-factor authentication set up and enforced, their users might have to perform the authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="ed35c-109">Okamžitého</span><span class="sxs-lookup"><span data-stu-id="ed35c-109">Instant-on</span></span>
<span data-ttu-id="ed35c-110">Toky spolupráce B2B nemůžeme přidat uživatele do adresáře a dynamicky aktualizovat během uplatnění pozvánku, aplikace přiřazení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ed35c-110">In the B2B collaboration flows, we add users to the directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="ed35c-111">Aktualizace a zápisy normálně dojít v jednom adresáři instance a musí být replikována napříč všemi instancemi.</span><span class="sxs-lookup"><span data-stu-id="ed35c-111">The updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="ed35c-112">Jakmile jsou aktualizovány všechny instance po dokončení replikace.</span><span class="sxs-lookup"><span data-stu-id="ed35c-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="ed35c-113">V některých případech, pokud objekt se zapisují nebo aktualizuje v jedné instance a volání k načtení tohoto objektu je k jiné instanci, může dojít latenci replikace.</span><span class="sxs-lookup"><span data-stu-id="ed35c-113">Sometimes when the object is written or updated in one instance and the call to retrieve this object is to another instance, replication latencies can occur.</span></span> <span data-ttu-id="ed35c-114">Pokud k tomu dojde, aktualizovat nebo opakujte pomohou.</span><span class="sxs-lookup"><span data-stu-id="ed35c-114">If that happens, refresh or retry to help.</span></span> <span data-ttu-id="ed35c-115">Pokud píšete mobilní aplikace pomocí našem rozhraní API, opakování s některé back vypnout je dobré, Obranným postupem se tím tento problém.</span><span class="sxs-lookup"><span data-stu-id="ed35c-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice to alleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed35c-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed35c-116">Next steps</span></span>

<span data-ttu-id="ed35c-117">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="ed35c-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ed35c-118">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ed35c-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ed35c-119">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ed35c-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="ed35c-120">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="ed35c-120">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="ed35c-121">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="ed35c-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="ed35c-122">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ed35c-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="ed35c-123">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed35c-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="ed35c-124">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ed35c-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="ed35c-125">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ed35c-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="ed35c-126">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="ed35c-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="ed35c-127">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="ed35c-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
