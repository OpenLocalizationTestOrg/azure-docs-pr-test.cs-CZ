---
title: "Použití skupin pro správu přístupu k prostředkům v Azure Active Directory | Microsoft Docs"
description: "Postup používání skupin v Azure Active Directory můžete spravovat přístup uživatelů k místní a cloudové aplikace a prostředky."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cd8125eda7643f0b190d35cbb89edf8b7b4eca30
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a><span data-ttu-id="51427-103">Spravovat přístup k prostředkům pomocí skupin Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="51427-103">Manage access to resources with Azure Active Directory groups</span></span>
<span data-ttu-id="51427-104">Azure Active Directory (Azure AD) je komplexní identit a přístupu řešení správy, poskytuje výkonnou sadu funkcí, které chcete spravovat přístup k místní a cloudové aplikace a prostředky, včetně služeb Microsoft online services třeba Office 365 a World aplikací Microsoft SaaS.</span><span class="sxs-lookup"><span data-stu-id="51427-104">Azure Active Directory (Azure AD) is a comprehensive identity and access management solution that provides a robust set of capabilities to manage access to on-premises and cloud applications and resources including Microsoft online services like Office 365 and a world of non-Microsoft SaaS applications.</span></span> <span data-ttu-id="51427-105">Tento článek obsahuje přehled, ale pokud chcete spustit pomocí služby Azure AD seskupení právě teď, postupujte podle pokynů v [Správa skupin zabezpečení ve službě Azure AD](active-directory-accessmanagement-manage-groups.md).</span><span class="sxs-lookup"><span data-stu-id="51427-105">This article provides an overview, but if you want to start using Azure AD groups right now, follow the instructions in [Managing security groups in Azure AD](active-directory-accessmanagement-manage-groups.md).</span></span> <span data-ttu-id="51427-106">Pokud chcete zobrazit, jak můžete použít PowerShell ke správě skupin ve službě Azure Active directory si můžete přečíst více v [rutiny služby Azure Active Directory pro správu skupin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).</span><span class="sxs-lookup"><span data-stu-id="51427-106">If you want to see how you can use PowerShell to manage groups in Azure Active directory you can read more in [Azure Active Directory cmdlets for group management](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="51427-107">Chcete-li používat Azure Active Directory, potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="51427-107">To use Azure Active Directory, you need an Azure account.</span></span> <span data-ttu-id="51427-108">Pokud účet nemáte, můžete [si zaregistrovat bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51427-108">If you don't have an account, you can [sign up for a free Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
>
>

<span data-ttu-id="51427-109">V rámci Azure AD mezi hlavní funkce je schopnost spravovat přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="51427-109">Within Azure AD, one of the major features is the ability to manage access to resources.</span></span> <span data-ttu-id="51427-110">Tyto prostředky můžou být součástí adresáři, jako v případě oprávnění ke správě objektů pomocí rolí v adresáři nebo prostředky, které jsou externí vzhledem k adresáři, jako je například aplikace SaaS, služby Azure a webů služby SharePoint nebo místních prostředků.</span><span class="sxs-lookup"><span data-stu-id="51427-110">These resources can be part of the directory, as in the case of permissions to manage objects through roles in the directory, or resources that are external to the directory, such as SaaS applications, Azure services, and SharePoint sites or on-premises resources.</span></span> <span data-ttu-id="51427-111">Existují čtyři způsoby, které uživatele lze přiřadit přístupová práva k prostředku:</span><span class="sxs-lookup"><span data-stu-id="51427-111">There are four ways a user can be assigned access rights to a resource:</span></span>

1. <span data-ttu-id="51427-112">Přímé přiřazení</span><span class="sxs-lookup"><span data-stu-id="51427-112">Direct assignment</span></span>

    <span data-ttu-id="51427-113">Vlastník prostředku může být přímo k prostředku přiřazena uživatelům.</span><span class="sxs-lookup"><span data-stu-id="51427-113">Users can be assigned directly to a resource by the owner of that resource.</span></span>
2. <span data-ttu-id="51427-114">Členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="51427-114">Group membership</span></span>

    <span data-ttu-id="51427-115">Skupinu můžete přiřadit k prostředku vlastníkem prostředku a díky tomu, udělení členové skupiny přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="51427-115">A group can be assigned to a resource by the resource owner, and by doing so, granting the members of that group access to the resource.</span></span> <span data-ttu-id="51427-116">Členství ve skupině potom můžete spravovat vlastníkem skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-116">Membership of the group can then be managed by the owner of the group.</span></span> <span data-ttu-id="51427-117">Vlastník prostředku deleguje prakticky oprávnění přiřazovat uživatele k jeho prostředku vlastníkovi skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-117">Effectively, the resource owner delegates the permission to assign users to their resource to the owner of the group.</span></span>
3. <span data-ttu-id="51427-118">Založený na pravidlech</span><span class="sxs-lookup"><span data-stu-id="51427-118">Rule-based</span></span>

    <span data-ttu-id="51427-119">Vlastník prostředku může použít pravidlo k express, kteří uživatelé by se měla přiřadit přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="51427-119">The resource owner can use a rule to express which users should be assigned access to a resource.</span></span> <span data-ttu-id="51427-120">Výsledek pravidlo závisí na atributy používané v tomto pravidle a jejich hodnoty pro konkrétní uživatele, a díky tomu vlastník prostředku deleguje efektivně právo spravovat přístup k jejich prostředek pro autoritativní zdroj pro atributy, které se používají v Toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="51427-120">The outcome of the rule depends on the attributes used in that rule and their values for specific users, and by doing so, the resource owner effectively delegates the right to manage access to their resource to the authoritative source for the attributes that are used in the rule.</span></span> <span data-ttu-id="51427-121">Vlastník prostředku stále spravuje pravidlo samotné a určí, které atributy a hodnoty poskytovat přístup k jejich prostředkům.</span><span class="sxs-lookup"><span data-stu-id="51427-121">The resource owner still manages the rule itself and determines which attributes and values provide access to their resource.</span></span>
4. <span data-ttu-id="51427-122">Externí autority</span><span class="sxs-lookup"><span data-stu-id="51427-122">External authority</span></span>

    <span data-ttu-id="51427-123">Přístup k prostředku je odvozený z externího zdroje; například skupina, která se synchronizují z autoritativní zdroj například místního adresáře nebo SaaS aplikace například WorkDay.</span><span class="sxs-lookup"><span data-stu-id="51427-123">The access to a resource is derived from an external source; for example, a group that is synchronized from an authoritative source such as an on-premises directory or a SaaS app such as WorkDay.</span></span> <span data-ttu-id="51427-124">Vlastník prostředku přiřadí skupinu a poskytne přístup k prostředku a externí zdroj spravuje členy skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-124">The resource owner assigns the group to provide access to the resource, and the external source manages the members of the group.</span></span>

   ![Přehled diagram správy přístupu](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a><span data-ttu-id="51427-126">Přehrát video, které popisuje správu přístupu</span><span class="sxs-lookup"><span data-stu-id="51427-126">Watch a video that explains access management</span></span>
<span data-ttu-id="51427-127">Podívejte se na krátké video, které vysvětluje více o toto:</span><span class="sxs-lookup"><span data-stu-id="51427-127">You can watch a short video that explains more about this:</span></span>

<span data-ttu-id="51427-128">**Azure AD: Úvod do dynamické členství ve skupinách**</span><span class="sxs-lookup"><span data-stu-id="51427-128">**Azure AD: Introduction to dynamic membership for groups**</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a><span data-ttu-id="51427-129">Jak Správa v Azure Active Directory pracovní přístupu?</span><span class="sxs-lookup"><span data-stu-id="51427-129">How does access management in Azure Active Directory work?</span></span>
<span data-ttu-id="51427-130">V centru Azure AD je řešení pro správu přístupu skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="51427-130">At the center of the Azure AD access management solution is the security group.</span></span> <span data-ttu-id="51427-131">Pomocí skupiny zabezpečení ke správě přístupu k prostředkům je dobře známé zlepší, která umožňuje flexibilní a snadno srozumitelné způsob, jak poskytnout přístup k prostředku pro zamýšlené skupiny uživatelů.</span><span class="sxs-lookup"><span data-stu-id="51427-131">Using a security group to manage access to resources is a well-known paradigm, which allows for a flexible and easily understood way to provide access to a resource for the intended group of users.</span></span> <span data-ttu-id="51427-132">Vlastník prostředku (nebo správce adresáře) můžete přiřadit skupiny k poskytování určité přístupu k prostředkům, které vlastní.</span><span class="sxs-lookup"><span data-stu-id="51427-132">The resource owner (or the administrator of the directory) can assign a group to provide a certain access right to the resources they own.</span></span> <span data-ttu-id="51427-133">Členové skupiny se poskytnout přístup a vlastníka prostředku můžete udělit oprávnění ke správě seznamu členů skupiny jinému uživateli, jako je například oddělení správce nebo správce technické podpory.</span><span class="sxs-lookup"><span data-stu-id="51427-133">The members of the group will be provided the access, and the resource owner can delegate the right to manage the members list of a group to someone else, such as a department manager or a helpdesk administrator.</span></span>

![Diagram správy přístupu služby Azure Active Directory](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

<span data-ttu-id="51427-135">Vlastník skupiny, můžete zpřístupnit této skupiny pro samoobslužné služby požadavky.</span><span class="sxs-lookup"><span data-stu-id="51427-135">The owner of a group can also make that group available for self-service requests.</span></span> <span data-ttu-id="51427-136">Tím, koncový uživatel může hledat a najít skupinu a podání žádosti o připojení, efektivně vyhledávání oprávnění pro přístup k prostředkům, které jsou spravované prostřednictvím skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-136">In doing so, an end user can search for and find the group and make a request to join, effectively seeking permission to access the resources that are managed through the group.</span></span> <span data-ttu-id="51427-137">Vlastník skupiny můžete nastavit skupiny tak, aby požadavků spojení budou automaticky schváleny nebo vyžadovat schválení vlastníka skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-137">The owner of the group can set up the group so that join requests are approved automatically or require approval by the owner of the group.</span></span> <span data-ttu-id="51427-138">Když uživatel odešle žádost o připojení ke skupině, žádost o připojení se předají na vlastníky skupiny.</span><span class="sxs-lookup"><span data-stu-id="51427-138">When a user makes a request to join a group, the join request is forwarded to the owners of the group.</span></span> <span data-ttu-id="51427-139">Pokud jeden z vlastníci schválí požadavek, je upozornění žádajícího uživatele a uživatel je připojený ke skupině.</span><span class="sxs-lookup"><span data-stu-id="51427-139">If one of the owners approves the request, the requesting user is notified and the user is joined to the group.</span></span> <span data-ttu-id="51427-140">Pokud jeden z vlastníci odmítne požadavek, je žádajícího uživatele oznámení, ale není připojen ke skupině.</span><span class="sxs-lookup"><span data-stu-id="51427-140">If one of the owners denies the request, the requesting user is notified but not joined to the group.</span></span>

## <a name="getting-started-with-access-management"></a><span data-ttu-id="51427-141">Začínáme se správou přístupu</span><span class="sxs-lookup"><span data-stu-id="51427-141">Getting started with access management</span></span>
<span data-ttu-id="51427-142">Jste připravení začít?</span><span class="sxs-lookup"><span data-stu-id="51427-142">Ready to get started?</span></span> <span data-ttu-id="51427-143">Vyzkoušejte si základní úlohy, které můžete provést pomocí skupin Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51427-143">You should try out some of the basic tasks you can do with Azure AD groups.</span></span> <span data-ttu-id="51427-144">Použijte tyto možnosti zajistit specializované přístup na různé skupiny uživatelů pro různé prostředky ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="51427-144">Use these capabilities to provide specialized access to different groups of people for different resources in your organization.</span></span> <span data-ttu-id="51427-145">Seznam základní první kroky jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="51427-145">A list of basic first steps are listed below.</span></span>

* [<span data-ttu-id="51427-146">Vytvoření jednoduché pravidlo konfigurace dynamické členství ve skupině</span><span class="sxs-lookup"><span data-stu-id="51427-146">Creating a simple rule to configure dynamic memberships for a group</span></span>](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)
* [<span data-ttu-id="51427-147">Pomocí skupiny pro správu přístupu k aplikacím SaaS</span><span class="sxs-lookup"><span data-stu-id="51427-147">Using a group to manage access to SaaS applications</span></span>](active-directory-accessmanagement-group-saasapps.md)
* [<span data-ttu-id="51427-148">Zpřístupnění skupiny pro koncového uživatele samoobslužné služby</span><span class="sxs-lookup"><span data-stu-id="51427-148">Making a group available for end user self-service</span></span>](active-directory-accessmanagement-self-service-group-management.md)
* [<span data-ttu-id="51427-149">Synchronizuje se místní skupině do Azure pomocí Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="51427-149">Syncing an on-premises group to Azure using Azure AD Connect</span></span>](active-directory-aadconnect.md)
* [<span data-ttu-id="51427-150">Správa vlastníků pro skupinu</span><span class="sxs-lookup"><span data-stu-id="51427-150">Managing owners for a group</span></span>](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a><span data-ttu-id="51427-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51427-151">Next steps</span></span>
<span data-ttu-id="51427-152">Teď, když porozuměl základní informace o správu přístupu tady jsou některé další rozšířené možnosti dostupné v Azure Active Directory pro správu přístupu k aplikacím a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="51427-152">Now that you have understood the basics of access management, here are some additional advanced capabilities available in Azure Active Directory for managing access to your applications and resources.</span></span>

* [<span data-ttu-id="51427-153">Vytváření rozšířených pravidel pomocí atributů</span><span class="sxs-lookup"><span data-stu-id="51427-153">Using attributes to create advanced rules</span></span>](active-directory-accessmanagement-groups-with-advanced-rules.md)
* [<span data-ttu-id="51427-154">Správa skupin zabezpečení ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="51427-154">Managing security groups in Azure AD</span></span>](active-directory-accessmanagement-manage-groups.md)
* [<span data-ttu-id="51427-155">Nastavení vyhrazené skupiny ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="51427-155">Setting up dedicated groups in Azure AD</span></span>](active-directory-accessmanagement-dedicated-groups.md)
* [<span data-ttu-id="51427-156">Referenční dokumentace rozhraní Graph API pro skupiny</span><span class="sxs-lookup"><span data-stu-id="51427-156">Graph API reference for groups</span></span>](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [<span data-ttu-id="51427-157">Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny</span><span class="sxs-lookup"><span data-stu-id="51427-157">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)