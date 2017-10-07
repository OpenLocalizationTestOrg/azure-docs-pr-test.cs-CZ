---
title: "aaaDynamic skupin a spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B lze použít s dynamickými skupinami Azure AD"
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="251a5-103">Dynamické skupiny a spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="251a5-104">Co jsou dynamické skupiny?</span><span class="sxs-lookup"><span data-stu-id="251a5-104">What are dynamic groups?</span></span>
<span data-ttu-id="251a5-105">Konfigurace dynamické členství ve skupině zabezpečení pro Azure Active Directory (Azure AD) je k dispozici v [hello portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="251a5-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [hello Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="251a5-106">Správci mohou nastavit pravidla, která toopopulate skupin, které jsou vytvořené v Azure Active Directory založit na atributech uživatelů (například userType, oddělení nebo země).</span><span class="sxs-lookup"><span data-stu-id="251a5-106">Administrators can set rules toopopulate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="251a5-107">Členové můžete automaticky přidají tooor odebrat ze skupiny zabezpečení na základě jejich atributů.</span><span class="sxs-lookup"><span data-stu-id="251a5-107">Members can be automatically added tooor removed from a security group based on their attributes.</span></span> <span data-ttu-id="251a5-108">Tyto skupiny může poskytovat přístup k tooapplications nebo cloudové prostředky (webů služby SharePoint, dokumenty) a tooassign licence toomembers.</span><span class="sxs-lookup"><span data-stu-id="251a5-108">These groups can provide access tooapplications or cloud resources (SharePoint sites, documents) and tooassign licenses toomembers.</span></span> <span data-ttu-id="251a5-109">Další informace o dynamických skupin v [vyhrazené skupiny ve službě Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="251a5-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="251a5-110">Hello odpovídající [licencování Azure AD Premium P1 a P2](https://azure.microsoft.com/pricing/details/active-directory/) je požadovaná toocreate a používání dynamických skupin.</span><span class="sxs-lookup"><span data-stu-id="251a5-110">hello appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required toocreate and use dynamic groups.</span></span> <span data-ttu-id="251a5-111">Další informace najdete v článku hello [vytvořit pravidla založená na atributu pro dynamické členství ve skupině v Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="251a5-111">Learn more in hello article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-hello-built-in-dynamic-groups"></a><span data-ttu-id="251a5-112">Co jsou hello integrované dynamické skupiny?</span><span class="sxs-lookup"><span data-stu-id="251a5-112">What are hello built-in dynamic groups?</span></span>
<span data-ttu-id="251a5-113">Hello **všichni uživatelé** dynamická skupina umožňuje toocreate Správci klienta skupina obsahující všechny uživatele v klientovi hello s jedním, klikněte na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="251a5-113">hello **All users** dynamic group enables tenant admins toocreate a group containing all users in hello tenant with a single click.</span></span> <span data-ttu-id="251a5-114">Ve výchozím nastavení, hello **všichni uživatelé** skupina obsahuje všechny uživatele v adresáři hello, včetně členů a hostů.</span><span class="sxs-lookup"><span data-stu-id="251a5-114">By default, hello **All users** group includes all users in hello directory, including Members and Guests.</span></span>
<span data-ttu-id="251a5-115">V rámci hello nového portálu správce Azure Active Directory, můžete zvolit tooenable hello **všichni uživatelé** v nastavení skupiny zobrazit hello.</span><span class="sxs-lookup"><span data-stu-id="251a5-115">Within hello new Azure Active Directory admin portal, you can choose tooenable hello **All users** group in hello Group Settings view.</span></span>

![předdefinované skupiny](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a><span data-ttu-id="251a5-117">Posílení zabezpečení hello všechny dynamické skupiny uživatelů</span><span class="sxs-lookup"><span data-stu-id="251a5-117">Hardening hello All users dynamic group</span></span>
<span data-ttu-id="251a5-118">Ve výchozím nastavení, hello **všichni uživatelé** skupina obsahuje také uživatelům (Host) spolupráce B2B.</span><span class="sxs-lookup"><span data-stu-id="251a5-118">By default, hello **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="251a5-119">Dále je možné zabezpečit vaše **všichni uživatelé** skupiny pomocí uživatele typu Host tooremove pravidla.</span><span class="sxs-lookup"><span data-stu-id="251a5-119">You can further secure your **All users** group by using a rule tooremove guest users.</span></span> <span data-ttu-id="251a5-120">Hello následující obrázek znázorňuje hello **všichni uživatelé** skupinu upravit tooexclude hostů.</span><span class="sxs-lookup"><span data-stu-id="251a5-120">hello following illustration shows hello **All users** group modified tooexclude guests.</span></span>

![Povolit všechny skupiny uživatelů](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="251a5-122">Může také pro vás užitečné toocreate nové dynamické skupiny, který obsahuje pouze uživatele typu Host, takže můžete použít toothem zásady (například zásady Azure AD podmíněného přístupu).</span><span class="sxs-lookup"><span data-stu-id="251a5-122">You might also find it useful toocreate a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) toothem.</span></span>
<span data-ttu-id="251a5-123">Jak taková skupina může vypadat:</span><span class="sxs-lookup"><span data-stu-id="251a5-123">What such a group might look like:</span></span>

![vyloučit uživatele typu Host](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="251a5-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="251a5-125">Next steps</span></span>

<span data-ttu-id="251a5-126">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="251a5-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="251a5-127">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="251a5-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="251a5-128">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="251a5-129">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-129">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="251a5-130">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="251a5-131">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="251a5-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="251a5-132">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="251a5-133">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="251a5-134">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="251a5-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="251a5-135">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="251a5-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="251a5-136">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="251a5-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
