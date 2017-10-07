---
title: "aaaDelegate pozvánky pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Vlastnosti uživatelů služby Active Directory s B2B spolupráce se dají konfigurovat"
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
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="db60f-103">Delegát pozvánky pro spolupráci Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="db60f-104">Se spoluprací business-to-business (B2B) Azure Active Directory (Azure AD) nemáte toobe pozvánek toosend globálního správce.</span><span class="sxs-lookup"><span data-stu-id="db60f-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have toobe a global admin toosend invitations.</span></span> <span data-ttu-id="db60f-105">Místo toho můžete použít zásady a delegovat pozvánek toousers, jejichž role mohly toosend pozvánky.</span><span class="sxs-lookup"><span data-stu-id="db60f-105">Instead, you can use policies and delegate invitations toousers whose roles allow them toosend invitations.</span></span> <span data-ttu-id="db60f-106">Důležité nový způsob toodelegate hosta uživatele pozvánek je prostřednictvím hello pozvání hosta odeslal role.</span><span class="sxs-lookup"><span data-stu-id="db60f-106">An important new way toodelegate guest user invitations is through hello Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="db60f-107">Pozvání hosta odeslal role</span><span class="sxs-lookup"><span data-stu-id="db60f-107">Guest Inviter role</span></span>
<span data-ttu-id="db60f-108">Jsme můžete přiřadit uživatele tooGuest hello pozval vás role toosend pozvánky.</span><span class="sxs-lookup"><span data-stu-id="db60f-108">We can assign hello user tooGuest Inviter role toosend invitations.</span></span> <span data-ttu-id="db60f-109">Nemáte toobe členem pozvánek toosend roli globálního správce hello.</span><span class="sxs-lookup"><span data-stu-id="db60f-109">You don't have toobe member of hello global admin role toosend invitations.</span></span> <span data-ttu-id="db60f-110">Ve výchozím nastavení můžete běžní uživatelé také vyvolat API pozvání hello, pokud není globální správce zakázáno pozvánky k běžní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="db60f-110">By default, regular users can also invoke hello invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="db60f-111">Uživatele můžete také vyvolat hello rozhraní API pomocí hello portál Azure nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="db60f-111">A user can also invoke hello API using hello Azure portal or PowerShell.</span></span>

<span data-ttu-id="db60f-112">Tady je příklad, který ukazuje, jak toouse prostředí PowerShell tooadd roli uživatele toohello pozvání hosta odeslal:</span><span class="sxs-lookup"><span data-stu-id="db60f-112">Here's an example that shows how toouse PowerShell tooadd a user toohello Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="db60f-113">Ovládací prvek, který můžete pozvat</span><span class="sxs-lookup"><span data-stu-id="db60f-113">Control who can invite</span></span>

![Ovládací prvek jak tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="db60f-115">S spolupráce Azure AD B2B můžete nastavit správce tenanta hello následující pozvánku zásady:</span><span class="sxs-lookup"><span data-stu-id="db60f-115">With Azure AD B2B collaboration, a tenant admin can set hello following invitation policies:</span></span>

- <span data-ttu-id="db60f-116">Vypnout pozvánek</span><span class="sxs-lookup"><span data-stu-id="db60f-116">Turn off invitations</span></span>
- <span data-ttu-id="db60f-117">Můžete pozvat jenom správci a uživatelé v roli hosta pozvánky hello</span><span class="sxs-lookup"><span data-stu-id="db60f-117">Only admins and users in hello Guest Inviter role can invite</span></span>
- <span data-ttu-id="db60f-118">Můžete pozvat správci, hello pozvání hosta odeslal role a členů</span><span class="sxs-lookup"><span data-stu-id="db60f-118">Admins, hello Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="db60f-119">Všichni uživatelé, včetně hostů, můžete pozvat</span><span class="sxs-lookup"><span data-stu-id="db60f-119">All users, including guests, can invite</span></span>

<span data-ttu-id="db60f-120">Ve výchozím nastavení, klienti jsou nastavené příliš č. 4.</span><span class="sxs-lookup"><span data-stu-id="db60f-120">By default, tenants are set too#4.</span></span> <span data-ttu-id="db60f-121">(Všechny uživatele, včetně hostů, můžete uživatele pozvat, B2B.)</span><span class="sxs-lookup"><span data-stu-id="db60f-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="db60f-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db60f-122">Next steps</span></span>

<span data-ttu-id="db60f-123">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="db60f-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="db60f-124">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="db60f-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="db60f-125">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="db60f-126">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-126">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="db60f-127">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="db60f-128">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="db60f-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="db60f-129">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="db60f-130">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="db60f-131">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="db60f-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="db60f-132">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="db60f-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="db60f-133">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="db60f-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
