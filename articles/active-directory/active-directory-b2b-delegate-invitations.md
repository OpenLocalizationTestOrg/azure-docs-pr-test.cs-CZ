---
title: "Delegovat pozvánky pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
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
ms.openlocfilehash: 78613cc978b585a98d235245194c02371f7f3849
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="0d6f9-103">Delegát pozvánky pro spolupráci Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="0d6f9-104">Se spoluprací business-to-business (B2B) Azure Active Directory (Azure AD) není nutné být globálním správcem odeslat pozvánky.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have to be a global admin to send invitations.</span></span> <span data-ttu-id="0d6f9-105">Místo toho můžete použít zásady a delegovat pozvánek uživatelům, jejichž role umožňují odeslání pozvánky.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-105">Instead, you can use policies and delegate invitations to users whose roles allow them to send invitations.</span></span> <span data-ttu-id="0d6f9-106">Je důležité nové možné delegovat pozvánek uživatele guest prostřednictvím role pozvání hosta odeslal.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-106">An important new way to delegate guest user invitations is through the Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="0d6f9-107">Pozvání hosta odeslal role</span><span class="sxs-lookup"><span data-stu-id="0d6f9-107">Guest Inviter role</span></span>
<span data-ttu-id="0d6f9-108">Jsme můžete přiřadit uživatele k roli pozvání hosta odeslal odeslat pozvánky.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-108">We can assign the user to Guest Inviter role to send invitations.</span></span> <span data-ttu-id="0d6f9-109">Nemusíte být členem role globálního správce odeslat pozvánky.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-109">You don't have to be member of the global admin role to send invitations.</span></span> <span data-ttu-id="0d6f9-110">Ve výchozím nastavení můžete běžní uživatelé také vyvolat rozhraní API pozvání, pokud není globální správce zakázáno pozvánky k běžní uživatelé.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-110">By default, regular users can also invoke the invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="0d6f9-111">Uživatele můžete také vyvolat rozhraní API pomocí portálu Azure nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-111">A user can also invoke the API using the Azure portal or PowerShell.</span></span>

<span data-ttu-id="0d6f9-112">Tady je příklad, který ukazuje, jak přidat uživatele k roli pozvání hosta odeslal pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0d6f9-112">Here's an example that shows how to use PowerShell to add a user to the Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="0d6f9-113">Ovládací prvek, který můžete pozvat</span><span class="sxs-lookup"><span data-stu-id="0d6f9-113">Control who can invite</span></span>

![Řídí pozvat](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="0d6f9-115">S spolupráce Azure AD B2B správce klienta můžete nastavit tyto zásady pozvání:</span><span class="sxs-lookup"><span data-stu-id="0d6f9-115">With Azure AD B2B collaboration, a tenant admin can set the following invitation policies:</span></span>

- <span data-ttu-id="0d6f9-116">Vypnout pozvánek</span><span class="sxs-lookup"><span data-stu-id="0d6f9-116">Turn off invitations</span></span>
- <span data-ttu-id="0d6f9-117">Pouze správci a uživatelé v roli hosta pozvánky můžete pozvat</span><span class="sxs-lookup"><span data-stu-id="0d6f9-117">Only admins and users in the Guest Inviter role can invite</span></span>
- <span data-ttu-id="0d6f9-118">Můžete pozvat správci, pozvání hosta odeslal role a členů</span><span class="sxs-lookup"><span data-stu-id="0d6f9-118">Admins, the Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="0d6f9-119">Všichni uživatelé, včetně hostů, můžete pozvat</span><span class="sxs-lookup"><span data-stu-id="0d6f9-119">All users, including guests, can invite</span></span>

<span data-ttu-id="0d6f9-120">Ve výchozím nastavení jsou klienti nastavené na #4.</span><span class="sxs-lookup"><span data-stu-id="0d6f9-120">By default, tenants are set to #4.</span></span> <span data-ttu-id="0d6f9-121">(Všechny uživatele, včetně hostů, můžete uživatele pozvat, B2B.)</span><span class="sxs-lookup"><span data-stu-id="0d6f9-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d6f9-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d6f9-122">Next steps</span></span>

<span data-ttu-id="0d6f9-123">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="0d6f9-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="0d6f9-124">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="0d6f9-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="0d6f9-125">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="0d6f9-126">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="0d6f9-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="0d6f9-127">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="0d6f9-128">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d6f9-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="0d6f9-129">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="0d6f9-130">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="0d6f9-131">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="0d6f9-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="0d6f9-132">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="0d6f9-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="0d6f9-133">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="0d6f9-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
