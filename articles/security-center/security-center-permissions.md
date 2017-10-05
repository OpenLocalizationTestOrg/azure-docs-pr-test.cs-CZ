---
title: "Oprávnění v Azure Security Center | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure Security Center používá řízení přístupu na základě rolí k přiřazení oprávnění pro uživatele a identifikuje povolené akce pro každou roli."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 0aaa99dda44d2020afd3e841e84020eb4ff87a85
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="e68aa-103">Oprávnění v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e68aa-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="e68aa-104">Azure Security Center používá [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md). To poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md), které se dají v Azure přiřadit uživatelům, skupinám a službám.</span><span class="sxs-lookup"><span data-stu-id="e68aa-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned to users, groups, and services in Azure.</span></span>

<span data-ttu-id="e68aa-105">Security Center vyhodnocuje konfigurace vaše prostředky a identifikují problémy se zabezpečením a ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e68aa-105">Security Center assesses the configuration of your resources to identify security issues and vulnerabilities.</span></span> <span data-ttu-id="e68aa-106">V Centru zabezpečení zobrazí jenom informace týkající se prostředek v případě jsou přiřazenou roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo skupinu prostředků, které daný prostředek patří.</span><span class="sxs-lookup"><span data-stu-id="e68aa-106">In Security Center, you only see information related to a resource when you are assigned the role of Owner, Contributor, or Reader for the subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="e68aa-107">Kromě těchto rolí existují ve službě Security Center dvě specifické role:</span><span class="sxs-lookup"><span data-stu-id="e68aa-107">In addition to these roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="e68aa-108">**Zabezpečení čtečky**: uživatel, který patří do této role má zobrazení práva k Security Center.</span><span class="sxs-lookup"><span data-stu-id="e68aa-108">**Security Reader**: A user that belongs to this role has viewing rights to Security Center.</span></span> <span data-ttu-id="e68aa-109">Uživatel může zobrazit doporučení, výstrahy, zásady zabezpečení a stavy zabezpečení, ale nelze provádět změny.</span><span class="sxs-lookup"><span data-stu-id="e68aa-109">The user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="e68aa-110">**Správce zabezpečení**: uživatel, který patří do této role má stejné oprávnění jako čtečky zabezpečení a zda můžete taky aktualizovat zásady zabezpečení a zavření výstrah a doporučení.</span><span class="sxs-lookup"><span data-stu-id="e68aa-110">**Security Administrator**: A user that belongs to this role has the same rights as the Security Reader and can also update the security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="e68aa-111">Role zabezpečení, čtečky zabezpečení a správce zabezpečení, mají přístup pouze ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="e68aa-111">The security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="e68aa-112">Role zabezpečení nemají přístup do jiných oblastí služby Azure, jako je například úložiště, webové a mobilní telefon nebo Internet věcí.</span><span class="sxs-lookup"><span data-stu-id="e68aa-112">The security roles do not have access to other service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="e68aa-113">Role a povolených akcí</span><span class="sxs-lookup"><span data-stu-id="e68aa-113">Roles and allowed actions</span></span>

<span data-ttu-id="e68aa-114">Následující tabulka zobrazuje role a povolené akce v Centru zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e68aa-114">The following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="e68aa-115">X označuje, že je povolené akce pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="e68aa-115">An X indicates that the action is allowed for that role.</span></span>

| <span data-ttu-id="e68aa-116">Role</span><span class="sxs-lookup"><span data-stu-id="e68aa-116">Role</span></span> | <span data-ttu-id="e68aa-117">Upravit zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e68aa-117">Edit security policy</span></span> | <span data-ttu-id="e68aa-118">Použít doporučení zabezpečení pro prostředek</span><span class="sxs-lookup"><span data-stu-id="e68aa-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="e68aa-119">Zavření výstrahy a doporučení</span><span class="sxs-lookup"><span data-stu-id="e68aa-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="e68aa-120">Zobrazit výstrahy a doporučení</span><span class="sxs-lookup"><span data-stu-id="e68aa-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="e68aa-121">Vlastník předplatného</span><span class="sxs-lookup"><span data-stu-id="e68aa-121">Subscription Owner</span></span> | <span data-ttu-id="e68aa-122">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-122">X</span></span> | <span data-ttu-id="e68aa-123">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-123">X</span></span> | <span data-ttu-id="e68aa-124">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-124">X</span></span> | <span data-ttu-id="e68aa-125">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-125">X</span></span> |
| <span data-ttu-id="e68aa-126">Předplatné přispěvatele</span><span class="sxs-lookup"><span data-stu-id="e68aa-126">Subscription Contributor</span></span> | <span data-ttu-id="e68aa-127">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-127">X</span></span> | <span data-ttu-id="e68aa-128">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-128">X</span></span> | <span data-ttu-id="e68aa-129">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-129">X</span></span> | <span data-ttu-id="e68aa-130">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-130">X</span></span> |
| <span data-ttu-id="e68aa-131">Vlastník skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e68aa-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="e68aa-132">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-132">X</span></span> | -- | <span data-ttu-id="e68aa-133">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-133">X</span></span> |
| <span data-ttu-id="e68aa-134">Skupina prostředků přispěvatele</span><span class="sxs-lookup"><span data-stu-id="e68aa-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="e68aa-135">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-135">X</span></span> | -- | <span data-ttu-id="e68aa-136">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-136">X</span></span> |
| <span data-ttu-id="e68aa-137">Čtenář</span><span class="sxs-lookup"><span data-stu-id="e68aa-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="e68aa-138">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-138">X</span></span> |
| <span data-ttu-id="e68aa-139">Správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e68aa-139">Security Administrator</span></span> | <span data-ttu-id="e68aa-140">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-140">X</span></span> | -- | <span data-ttu-id="e68aa-141">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-141">X</span></span> | <span data-ttu-id="e68aa-142">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-142">X</span></span> |
| <span data-ttu-id="e68aa-143">Čtečka zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e68aa-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="e68aa-144">X</span><span class="sxs-lookup"><span data-stu-id="e68aa-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="e68aa-145">Doporučujeme přiřadit uživatelům tu nejvíc omezenou roli, kterou ke své práci potřebují.</span><span class="sxs-lookup"><span data-stu-id="e68aa-145">We recommend that you assign the least permissive role needed for users to complete their tasks.</span></span> <span data-ttu-id="e68aa-146">Například přiřadíte role Čtenář uživatelům, kteří potřebují jenom zobrazit informace o stavu zabezpečení prostředku, ale nemusí provádět žádné akce, třeba uplatňovat doporučení nebo upravovat zásady.</span><span class="sxs-lookup"><span data-stu-id="e68aa-146">For example, assign the Reader role to users who only need to view information about the security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="e68aa-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e68aa-147">Next steps</span></span>
<span data-ttu-id="e68aa-148">Tento článek vysvětlení, jak Security Center používá RBAC přiřadit oprávnění pro uživatele a identifikovat povolené akce pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="e68aa-148">This article explained how Security Center uses RBAC to assign permissions to users and identified the allowed actions for each role.</span></span> <span data-ttu-id="e68aa-149">Teď, když jste obeznámeni s přiřazení rolí, které jsou potřeba k monitorování stavu zabezpečení vašeho předplatného, upravit zásady zabezpečení a použijete doporučení, zjistěte, jak:</span><span class="sxs-lookup"><span data-stu-id="e68aa-149">Now that you're familiar with the role assignments needed to monitor the security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="e68aa-150">Nastavení zásad zabezpečení v Centru zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e68aa-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="e68aa-151">Správa doporučení zabezpečení v Centru zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e68aa-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="e68aa-152">Sledování stavu zabezpečení prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="e68aa-152">Monitor the security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="e68aa-153">Správa a reakce na výstrahy zabezpečení ve službě Security Center</span><span class="sxs-lookup"><span data-stu-id="e68aa-153">Manage and respond to security alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="e68aa-154">Sledování partnerských řešení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="e68aa-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
