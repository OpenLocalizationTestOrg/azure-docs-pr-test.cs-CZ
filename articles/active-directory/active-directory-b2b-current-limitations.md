---
title: "aaaLimitations spolupráce Azure Active Directory s B2B | Microsoft Docs"
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
ms.openlocfilehash: 322081f32fbacfe67ee1300993c7df1870e498bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="ed370-103">Omezení spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="ed370-104">Spolupráce Azure Active Directory (Azure AD) s B2B právě subjektu toohello omezení popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ed370-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject toohello limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="ed370-105">Možné double aplikace Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="ed370-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="ed370-106">S B2B Azure AD můžete vynutit ověřování Multi-Factor authentication v organizaci poskytující prostředky hello (hello pozvání organizace).</span><span class="sxs-lookup"><span data-stu-id="ed370-106">With Azure AD B2B, you can enforce multi-factor authentication at hello resource organization (hello inviting organization).</span></span> <span data-ttu-id="ed370-107">Hello důvody tohoto přístupu jsou podrobně popsané na [podmíněného přístupu pro uživatele spolupráce B2B](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="ed370-107">hello reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="ed370-108">Pokud partnera již obsahuje službu Multi-Factor authentication nastavit a vynutit, uživatelé pravděpodobně tooperform hello ověřování jednou v jejich domovskou organizaci a potom znovu za vaše.</span><span class="sxs-lookup"><span data-stu-id="ed370-108">If a partner already has multi-factor authentication set up and enforced, their users might have tooperform hello authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="ed370-109">Okamžitého</span><span class="sxs-lookup"><span data-stu-id="ed370-109">Instant-on</span></span>
<span data-ttu-id="ed370-110">V hello B2B spolupráce toky jsme přidat uživatele toohello adresář a dynamicky aktualizovat během uplatnění pozvánku, aplikace přiřazení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ed370-110">In hello B2B collaboration flows, we add users toohello directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="ed370-111">aktualizace Hello a zápisy normálně dojít v jednom adresáři instance a musí být replikována napříč všemi instancemi.</span><span class="sxs-lookup"><span data-stu-id="ed370-111">hello updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="ed370-112">Jakmile jsou aktualizovány všechny instance po dokončení replikace.</span><span class="sxs-lookup"><span data-stu-id="ed370-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="ed370-113">Někdy při hello objektu je zapsána nebo aktualizovány v jedné instance a hello volání tooretrieve tento objekt je tooanother instance latence replikace může dojít.</span><span class="sxs-lookup"><span data-stu-id="ed370-113">Sometimes when hello object is written or updated in one instance and hello call tooretrieve this object is tooanother instance, replication latencies can occur.</span></span> <span data-ttu-id="ed370-114">Pokud k tomu dojde, aktualizovat nebo opakujte toohelp.</span><span class="sxs-lookup"><span data-stu-id="ed370-114">If that happens, refresh or retry toohelp.</span></span> <span data-ttu-id="ed370-115">Pokud píšete mobilní aplikace pomocí našich API pak opakování s některými back vypnout je dobré, Obranným postupem tooalleviate tento problém.</span><span class="sxs-lookup"><span data-stu-id="ed370-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice tooalleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed370-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed370-116">Next steps</span></span>

<span data-ttu-id="ed370-117">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="ed370-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ed370-118">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ed370-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ed370-119">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="ed370-120">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-120">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="ed370-121">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="ed370-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="ed370-122">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="ed370-123">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed370-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="ed370-124">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="ed370-125">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ed370-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="ed370-126">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="ed370-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="ed370-127">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="ed370-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
