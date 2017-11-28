---
title: "role pro uživatele tooa aaaAdd Azure Active Directory s B2B spolupráce | Microsoft Docs"
description: "Přidání role uživatele tooa hostovaný v Azure Active Directory"
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
ms.date: 03/15/2017
ms.author: sasubram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccc58a0c8ecc73f8e79a8d827efdc0ff93846a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permissions-toousers-from-partner-organizations-in-your-azure-active-directory-tenant"></a><span data-ttu-id="23ff3-103">Udělení oprávnění toousers z partnerských organizací ve vašem klientu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23ff3-103">Grant permissions toousers from partner organizations in your Azure Active Directory tenant</span></span>

<span data-ttu-id="23ff3-104">Uživatelé spolupráce Azure Active Directory (Azure AD) s B2B se přidají jako hosta uživatelé toohello adresář a jsou ve výchozím nastavení omezuje oprávnění hostovaného v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="23ff3-104">Azure Active Directory (Azure AD) B2B collaboration users are added as guest users toohello directory, and guest permissions in hello directory are restricted by default.</span></span> <span data-ttu-id="23ff3-105">Vaší firmy může být nutné některé hosta uživatelé toofill vyšší oprávnění role ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="23ff3-105">Your business may need some guest users toofill higher-privilege roles in your organization.</span></span> <span data-ttu-id="23ff3-106">toosupport definování vyšší oprávnění role uživatele typu Host může být role přidané tooany požadavky, na základě potřeb vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="23ff3-106">toosupport defining higher-privilege roles, guest users can be added tooany roles you desire, based on your organization's needs.</span></span>

## <a name="default-role"></a><span data-ttu-id="23ff3-107">Výchozí role</span><span class="sxs-lookup"><span data-stu-id="23ff3-107">Default role</span></span>

![Výchozí role](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a><span data-ttu-id="23ff3-109">Role Globální správce</span><span class="sxs-lookup"><span data-stu-id="23ff3-109">Global Administrator Role</span></span>

![role Globální správce](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a><span data-ttu-id="23ff3-111">Role omezené správce</span><span class="sxs-lookup"><span data-stu-id="23ff3-111">Limited Administrator Role</span></span>

![role omezené správce](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a><span data-ttu-id="23ff3-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23ff3-113">Next steps</span></span>

<span data-ttu-id="23ff3-114">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="23ff3-114">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="23ff3-115">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="23ff3-115">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="23ff3-116">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="23ff3-116">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="23ff3-117">Delegovat pozvánek B2bB spolupráce</span><span class="sxs-lookup"><span data-stu-id="23ff3-117">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="23ff3-118">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="23ff3-118">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="23ff3-119">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="23ff3-119">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="23ff3-120">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="23ff3-120">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="23ff3-121">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="23ff3-121">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="23ff3-122">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="23ff3-122">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="23ff3-123">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="23ff3-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="23ff3-124">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="23ff3-124">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
