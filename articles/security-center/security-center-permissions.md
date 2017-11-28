---
title: aaaPermissions v Azure Security Center | Microsoft Docs
description: "Tento článek vysvětluje, jak Azure Security Center používá toousers oprávnění tooassign řízení přístupu podle rolí a identifikuje hello povolené akce pro každou roli."
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
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="b808c-103">Oprávnění v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b808c-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="b808c-104">Azure Security Center používá [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md), který poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md) , lze přiřadit toousers, skupiny a služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="b808c-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned toousers, groups, and services in Azure.</span></span>

<span data-ttu-id="b808c-105">Security Center vyhodnocuje hello konfiguraci problémy se zabezpečením tooidentify prostředky a ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b808c-105">Security Center assesses hello configuration of your resources tooidentify security issues and vulnerabilities.</span></span> <span data-ttu-id="b808c-106">V Centru zabezpečení, uvidíte jenom informace související s tooa prostředků, když jsou přiřazeny hello roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo prostředek skupiny hello, která daný prostředek patří.</span><span class="sxs-lookup"><span data-stu-id="b808c-106">In Security Center, you only see information related tooa resource when you are assigned hello role of Owner, Contributor, or Reader for hello subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="b808c-107">Přidání toothese role existují dvě konkrétní role Security Center:</span><span class="sxs-lookup"><span data-stu-id="b808c-107">In addition toothese roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="b808c-108">**Zabezpečení čtečky**: uživatel, který patří toothis role má zobrazení Center tooSecurity práva.</span><span class="sxs-lookup"><span data-stu-id="b808c-108">**Security Reader**: A user that belongs toothis role has viewing rights tooSecurity Center.</span></span> <span data-ttu-id="b808c-109">uživatel Hello můžete zobrazit doporučení, výstrahy, zásady zabezpečení a stavy zabezpečení, ale nelze provádět změny.</span><span class="sxs-lookup"><span data-stu-id="b808c-109">hello user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="b808c-110">**Správce zabezpečení**: uživatel, který patří toothis role má hello stejná práva jako hello čtečky zabezpečení a můžete také aktualizovat zásady zabezpečení hello a zavření výstrah a doporučení.</span><span class="sxs-lookup"><span data-stu-id="b808c-110">**Security Administrator**: A user that belongs toothis role has hello same rights as hello Security Reader and can also update hello security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="b808c-111">role zabezpečení Hello čtečky zabezpečení a správce zabezpečení, mají přístup pouze ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="b808c-111">hello security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="b808c-112">role zabezpečení Hello nemají přístup tooother služby oblasti Azure, jako je například úložiště, webové a mobilní telefon nebo Internet věcí.</span><span class="sxs-lookup"><span data-stu-id="b808c-112">hello security roles do not have access tooother service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="b808c-113">Role a povolených akcí</span><span class="sxs-lookup"><span data-stu-id="b808c-113">Roles and allowed actions</span></span>

<span data-ttu-id="b808c-114">Hello následující tabulka zobrazuje role a povolené akce v Centru zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b808c-114">hello following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="b808c-115">X označuje, že je hello akci povolit pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="b808c-115">An X indicates that hello action is allowed for that role.</span></span>

| <span data-ttu-id="b808c-116">Role</span><span class="sxs-lookup"><span data-stu-id="b808c-116">Role</span></span> | <span data-ttu-id="b808c-117">Upravit zásady zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b808c-117">Edit security policy</span></span> | <span data-ttu-id="b808c-118">Použít doporučení zabezpečení pro prostředek</span><span class="sxs-lookup"><span data-stu-id="b808c-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="b808c-119">Zavření výstrahy a doporučení</span><span class="sxs-lookup"><span data-stu-id="b808c-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="b808c-120">Zobrazit výstrahy a doporučení</span><span class="sxs-lookup"><span data-stu-id="b808c-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="b808c-121">Vlastník předplatného</span><span class="sxs-lookup"><span data-stu-id="b808c-121">Subscription Owner</span></span> | <span data-ttu-id="b808c-122">X</span><span class="sxs-lookup"><span data-stu-id="b808c-122">X</span></span> | <span data-ttu-id="b808c-123">X</span><span class="sxs-lookup"><span data-stu-id="b808c-123">X</span></span> | <span data-ttu-id="b808c-124">X</span><span class="sxs-lookup"><span data-stu-id="b808c-124">X</span></span> | <span data-ttu-id="b808c-125">X</span><span class="sxs-lookup"><span data-stu-id="b808c-125">X</span></span> |
| <span data-ttu-id="b808c-126">Předplatné přispěvatele</span><span class="sxs-lookup"><span data-stu-id="b808c-126">Subscription Contributor</span></span> | <span data-ttu-id="b808c-127">X</span><span class="sxs-lookup"><span data-stu-id="b808c-127">X</span></span> | <span data-ttu-id="b808c-128">X</span><span class="sxs-lookup"><span data-stu-id="b808c-128">X</span></span> | <span data-ttu-id="b808c-129">X</span><span class="sxs-lookup"><span data-stu-id="b808c-129">X</span></span> | <span data-ttu-id="b808c-130">X</span><span class="sxs-lookup"><span data-stu-id="b808c-130">X</span></span> |
| <span data-ttu-id="b808c-131">Vlastník skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b808c-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="b808c-132">X</span><span class="sxs-lookup"><span data-stu-id="b808c-132">X</span></span> | -- | <span data-ttu-id="b808c-133">X</span><span class="sxs-lookup"><span data-stu-id="b808c-133">X</span></span> |
| <span data-ttu-id="b808c-134">Skupina prostředků přispěvatele</span><span class="sxs-lookup"><span data-stu-id="b808c-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="b808c-135">X</span><span class="sxs-lookup"><span data-stu-id="b808c-135">X</span></span> | -- | <span data-ttu-id="b808c-136">X</span><span class="sxs-lookup"><span data-stu-id="b808c-136">X</span></span> |
| <span data-ttu-id="b808c-137">Čtenář</span><span class="sxs-lookup"><span data-stu-id="b808c-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="b808c-138">X</span><span class="sxs-lookup"><span data-stu-id="b808c-138">X</span></span> |
| <span data-ttu-id="b808c-139">Správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b808c-139">Security Administrator</span></span> | <span data-ttu-id="b808c-140">X</span><span class="sxs-lookup"><span data-stu-id="b808c-140">X</span></span> | -- | <span data-ttu-id="b808c-141">X</span><span class="sxs-lookup"><span data-stu-id="b808c-141">X</span></span> | <span data-ttu-id="b808c-142">X</span><span class="sxs-lookup"><span data-stu-id="b808c-142">X</span></span> |
| <span data-ttu-id="b808c-143">Čtečka zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b808c-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="b808c-144">X</span><span class="sxs-lookup"><span data-stu-id="b808c-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="b808c-145">Doporučujeme, abyste přiřadili hello omezenou roli potřebné pro uživatele toocomplete úkoly.</span><span class="sxs-lookup"><span data-stu-id="b808c-145">We recommend that you assign hello least permissive role needed for users toocomplete their tasks.</span></span> <span data-ttu-id="b808c-146">Například přiřadíte hello čtečky role toousers, kdo stačí tooview informace o stavu zabezpečení hello prostředku, ale nemusí provádět žádné akce, třeba uplatňovat doporučení nebo upravovat zásady.</span><span class="sxs-lookup"><span data-stu-id="b808c-146">For example, assign hello Reader role toousers who only need tooview information about hello security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="b808c-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b808c-147">Next steps</span></span>
<span data-ttu-id="b808c-148">Tento článek vysvětlení, jak Security Center používá toousers oprávnění RBAC tooassign a identifikovat hello povolené akce pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="b808c-148">This article explained how Security Center uses RBAC tooassign permissions toousers and identified hello allowed actions for each role.</span></span> <span data-ttu-id="b808c-149">Teď, když jste obeznámeni s přiřazení rolí hello potřeby toomonitor hello stavu zabezpečení vašeho předplatného, upravit zásady zabezpečení a použijete doporučení, zjistěte, jak:</span><span class="sxs-lookup"><span data-stu-id="b808c-149">Now that you're familiar with hello role assignments needed toomonitor hello security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="b808c-150">Nastavení zásad zabezpečení v Centru zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b808c-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="b808c-151">Správa doporučení zabezpečení v Centru zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b808c-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="b808c-152">Sledování stavu zabezpečení hello vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="b808c-152">Monitor hello security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="b808c-153">Spravovat a reagovat toosecurity výstrah v Security Center</span><span class="sxs-lookup"><span data-stu-id="b808c-153">Manage and respond toosecurity alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="b808c-154">Sledování partnerských řešení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b808c-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
