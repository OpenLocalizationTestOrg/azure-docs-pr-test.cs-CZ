---
title: "Azure Active Directory identitu ochrana – jak odblokovat uživatele | Microsoft Docs"
description: "Zjistěte, jak odblokovat uživatele, které byly blokována zásady služby Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochrany identit Azure active directory, odblokovat uživatele"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ce6b2805e7281dff7752a73ada86be11d7e01fc3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a><span data-ttu-id="558ac-104">Azure Active Directory identitu ochrana – jak odblokovat uživatele</span><span class="sxs-lookup"><span data-stu-id="558ac-104">Azure Active Directory Identity Protection - How to unblock users</span></span>
<span data-ttu-id="558ac-105">S Azure Active Directory Identity Protection můžete nakonfigurovat zásady pro blokování uživatele, pokud jsou splněny podmínky nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="558ac-105">With Azure Active Directory Identity Protection, you can configure policies to block users if the configured conditions are satisfied.</span></span> <span data-ttu-id="558ac-106">Obvykle blokovaný uživatel kontakty technické podpory k odblokování.</span><span class="sxs-lookup"><span data-stu-id="558ac-106">Typically, a blocked user contacts help desk to become unblocked.</span></span> <span data-ttu-id="558ac-107">Tato témata vysvětluje kroky můžete provádět, abyste odblokovat blokovaný uživatel.</span><span class="sxs-lookup"><span data-stu-id="558ac-107">This topics explains the steps you can perform to unblock a blocked user.</span></span>

## <a name="determine-the-reason-for-blocking"></a><span data-ttu-id="558ac-108">Určete důvod pro blokování</span><span class="sxs-lookup"><span data-stu-id="558ac-108">Determine the reason for blocking</span></span>
<span data-ttu-id="558ac-109">Jako první krok odblokovat uživatele budete muset určit typ zásady, které má uživatel blokován, protože na něm závisí další kroky.</span><span class="sxs-lookup"><span data-stu-id="558ac-109">As a first step to unblock a user, you need to determine the type of policy that has blocked the user because your next steps are depending on it.</span></span>
<span data-ttu-id="558ac-110">S Azure Active Directory Identity Protection může se uživatel buď blokovaný přihlášení riziko zásadu nebo zásady riziko uživatele.</span><span class="sxs-lookup"><span data-stu-id="558ac-110">With Azure Active Directory Identity Protection, a user can be either blocked by a sign-in risk policy or a user risk policy.</span></span>

<span data-ttu-id="558ac-111">Můžete získat typ zásady, který má blokovaný uživatel z záhlaví v dialogu, který byl předložený uživateli při pokusu o přihlášení:</span><span class="sxs-lookup"><span data-stu-id="558ac-111">You can get the type of policy that has blocked a user from the heading in the dialog that was presented to the user during a sign-in attempt:</span></span>

| <span data-ttu-id="558ac-112">Zásada</span><span class="sxs-lookup"><span data-stu-id="558ac-112">Policy</span></span> | <span data-ttu-id="558ac-113">Dialogové okno uživatele</span><span class="sxs-lookup"><span data-stu-id="558ac-113">User dialog</span></span> |
| --- | --- |
| <span data-ttu-id="558ac-114">Riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="558ac-114">Sign-in risk</span></span> |![Blokované přihlášení](./media/active-directory-identityprotection-unblock-howto/02.png) |
| <span data-ttu-id="558ac-116">Riziko pro uživatele</span><span class="sxs-lookup"><span data-stu-id="558ac-116">User risk</span></span> |![Zablokovaný účet](./media/active-directory-identityprotection-unblock-howto/104.png) |

<span data-ttu-id="558ac-118">Uživatel, který je blokována:</span><span class="sxs-lookup"><span data-stu-id="558ac-118">A user that is blocked by:</span></span>

* <span data-ttu-id="558ac-119">Zásady přihlášení riziko je také označován jako podezřelou přihlášení</span><span class="sxs-lookup"><span data-stu-id="558ac-119">A sign-in risk policy is also known as suspicious sign-in</span></span>
* <span data-ttu-id="558ac-120">Zásady uživatele riziko je také označován jako účet v ohrožení</span><span class="sxs-lookup"><span data-stu-id="558ac-120">A user risk policy is also known as an account at risk</span></span>

## <a name="unblocking-suspicious-sign-ins"></a><span data-ttu-id="558ac-121">Odblokování podezřelé přihlášení</span><span class="sxs-lookup"><span data-stu-id="558ac-121">Unblocking suspicious sign-ins</span></span>
<span data-ttu-id="558ac-122">Chcete-li odblokovat podezřelé přihlášení, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="558ac-122">To unblock a suspicious sign-in, you have the following options:</span></span>

1. <span data-ttu-id="558ac-123">**Přihlášení ze zařízení nebo známé umístění** -běžným důvodem pro blokované podezřelé přihlášení jsou pokusů o přihlášení z neznámých míst nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="558ac-123">**Sign-in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices.</span></span> <span data-ttu-id="558ac-124">Vaši uživatelé můžete rychle zjistit, zda je z důvodu blokování tak, že zkusíte přihlášení ze zařízení nebo známé umístění.</span><span class="sxs-lookup"><span data-stu-id="558ac-124">Your users can quickly determine whether this is the blocking reason by trying to sign-in from a familiar location or device.</span></span>
2. <span data-ttu-id="558ac-125">**Vyloučit ze zásad** – Pokud se domníváte, že aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit uživatele z něj.</span><span class="sxs-lookup"><span data-stu-id="558ac-125">**Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.</span></span> <span data-ttu-id="558ac-126">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="558ac-126">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
3. <span data-ttu-id="558ac-127">**Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat zásadu.</span><span class="sxs-lookup"><span data-stu-id="558ac-127">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.</span></span> <span data-ttu-id="558ac-128">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="558ac-128">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="unblocking-accounts-at-risk"></a><span data-ttu-id="558ac-129">Odblokování účty v ohrožení</span><span class="sxs-lookup"><span data-stu-id="558ac-129">Unblocking accounts at risk</span></span>
<span data-ttu-id="558ac-130">Chcete-li odblokovat účtu ohrožený, máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="558ac-130">To unblock an account at risk, you have the following options:</span></span>

1. <span data-ttu-id="558ac-131">**Resetovat heslo** -resetujete heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="558ac-131">**Reset password** - You can reset the user's password.</span></span> <span data-ttu-id="558ac-132">V tématu [ruční zabezpečené heslo resetovat](active-directory-identityprotection.md#manual-secure-password-reset) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="558ac-132">See [manual secure password reset](active-directory-identityprotection.md#manual-secure-password-reset) for more details.</span></span>
2. <span data-ttu-id="558ac-133">**Zavřít všechny události riziko** – na uživatel bloky zásad riziko uživatele Pokud uživatel nakonfigurovaný úroveň blokování přístupu rizika byl dosažen.</span><span class="sxs-lookup"><span data-stu-id="558ac-133">**Dismiss all risk events** - The user risk policy blocks a user if the configured user risk level for blocking access has been reached.</span></span> <span data-ttu-id="558ac-134">Můžete snížit uživatele je úroveň rizika ručně ukončením hlášené rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="558ac-134">You can reduce a user's risk level by manually closing reported risk events.</span></span> <span data-ttu-id="558ac-135">Další podrobnosti najdete v tématu [zavření rizikových událostí ručně](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="558ac-135">For more details, see [closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>
3. <span data-ttu-id="558ac-136">**Vyloučit ze zásad** – Pokud se domníváte, že aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit uživatele z něj.</span><span class="sxs-lookup"><span data-stu-id="558ac-136">**Exclude from policy** - If you think that the current configuration of your sign-in policy is causing issues for specific users, you can exclude the users from it.</span></span> <span data-ttu-id="558ac-137">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="558ac-137">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
4. <span data-ttu-id="558ac-138">**Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat zásadu.</span><span class="sxs-lookup"><span data-stu-id="558ac-138">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable the policy.</span></span> <span data-ttu-id="558ac-139">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="558ac-139">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="558ac-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="558ac-140">Next steps</span></span>
 <span data-ttu-id="558ac-141">Opravdu chcete získat další informace o Azure AD Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="558ac-141">Do you want to know more about Azure AD Identity Protection?</span></span> <span data-ttu-id="558ac-142">Podívejte se na [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="558ac-142">Check out [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>
