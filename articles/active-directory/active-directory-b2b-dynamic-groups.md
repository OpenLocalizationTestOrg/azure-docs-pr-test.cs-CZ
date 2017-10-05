---
title: "Dynamické skupiny a spolupráce Azure Active Directory s B2B | Microsoft Docs"
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
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="1d69c-103">Dynamické skupiny a spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="1d69c-104">Co jsou dynamické skupiny?</span><span class="sxs-lookup"><span data-stu-id="1d69c-104">What are dynamic groups?</span></span>
<span data-ttu-id="1d69c-105">Konfigurace dynamické členství ve skupině zabezpečení pro Azure Active Directory (Azure AD) je k dispozici v [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1d69c-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [the Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1d69c-106">Správci mohou nastavit pravidla pro naplnění skupin, které jsou vytvořené v Azure Active Directory založit na atributech uživatelů (například userType, oddělení nebo země).</span><span class="sxs-lookup"><span data-stu-id="1d69c-106">Administrators can set rules to populate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="1d69c-107">Členové dají automaticky přidat nebo odebrat ze skupiny zabezpečení na základě jejich atributů.</span><span class="sxs-lookup"><span data-stu-id="1d69c-107">Members can be automatically added to or removed from a security group based on their attributes.</span></span> <span data-ttu-id="1d69c-108">Tyto skupiny můžete poskytnout přístup k aplikacím nebo prostředkům cloudu (webů služby SharePoint, dokumenty) a přiřadit licence na členy.</span><span class="sxs-lookup"><span data-stu-id="1d69c-108">These groups can provide access to applications or cloud resources (SharePoint sites, documents) and to assign licenses to members.</span></span> <span data-ttu-id="1d69c-109">Další informace o dynamických skupin v [vyhrazené skupiny ve službě Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="1d69c-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="1d69c-110">Odpovídající [licencování Azure AD Premium P1 a P2](https://azure.microsoft.com/pricing/details/active-directory/) je nutná k vytváření a použití dynamických skupin.</span><span class="sxs-lookup"><span data-stu-id="1d69c-110">The appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required to create and use dynamic groups.</span></span> <span data-ttu-id="1d69c-111">Další informace najdete v článku [vytvořit pravidla založená na atributu pro dynamické členství ve skupině v Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1d69c-111">Learn more in the article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-the-built-in-dynamic-groups"></a><span data-ttu-id="1d69c-112">Co jsou integrované dynamické skupiny?</span><span class="sxs-lookup"><span data-stu-id="1d69c-112">What are the built-in dynamic groups?</span></span>
<span data-ttu-id="1d69c-113">**Všichni uživatelé** dynamická skupina umožňuje správci klientů k vytvoření skupiny obsahující všechny uživatele v klientovi s jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="1d69c-113">The **All users** dynamic group enables tenant admins to create a group containing all users in the tenant with a single click.</span></span> <span data-ttu-id="1d69c-114">Ve výchozím nastavení **všichni uživatelé** skupina obsahuje všechny uživatele v adresáři, včetně členů a hostů.</span><span class="sxs-lookup"><span data-stu-id="1d69c-114">By default, the **All users** group includes all users in the directory, including Members and Guests.</span></span>
<span data-ttu-id="1d69c-115">V rámci nový portál pro správu Azure Active Directory, můžete povolit **všichni uživatelé** skupiny v nastavení skupiny zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1d69c-115">Within the new Azure Active Directory admin portal, you can choose to enable the **All users** group in the Group Settings view.</span></span>

![předdefinované skupiny](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a><span data-ttu-id="1d69c-117">Posílení zabezpečení všech dynamické skupiny uživatelů</span><span class="sxs-lookup"><span data-stu-id="1d69c-117">Hardening the All users dynamic group</span></span>
<span data-ttu-id="1d69c-118">Ve výchozím nastavení **všichni uživatelé** skupina obsahuje také uživatelům (Host) spolupráce B2B.</span><span class="sxs-lookup"><span data-stu-id="1d69c-118">By default, the **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="1d69c-119">Dále je možné zabezpečit vaše **všichni uživatelé** skupiny pomocí pravidla odebrat uživatele typu Host.</span><span class="sxs-lookup"><span data-stu-id="1d69c-119">You can further secure your **All users** group by using a rule to remove guest users.</span></span> <span data-ttu-id="1d69c-120">Následující obrázek ukazuje **všichni uživatelé** skupiny upravit tak, aby vyloučit hostů.</span><span class="sxs-lookup"><span data-stu-id="1d69c-120">The following illustration shows the **All users** group modified to exclude guests.</span></span>

![Povolit všechny skupiny uživatelů](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="1d69c-122">Může také pro vás užitečné k vytvoření nové dynamické skupiny, která obsahuje pouze uživatele typu Host, tak, aby mohli použít zásady (například zásady Azure AD podmíněného přístupu) k nim.</span><span class="sxs-lookup"><span data-stu-id="1d69c-122">You might also find it useful to create a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) to them.</span></span>
<span data-ttu-id="1d69c-123">Jak taková skupina může vypadat:</span><span class="sxs-lookup"><span data-stu-id="1d69c-123">What such a group might look like:</span></span>

![vyloučit uživatele typu Host](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="1d69c-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d69c-125">Next steps</span></span>

<span data-ttu-id="1d69c-126">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="1d69c-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="1d69c-127">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="1d69c-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="1d69c-128">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="1d69c-129">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="1d69c-129">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="1d69c-130">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="1d69c-131">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d69c-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="1d69c-132">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="1d69c-133">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="1d69c-134">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="1d69c-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="1d69c-135">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="1d69c-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="1d69c-136">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="1d69c-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
