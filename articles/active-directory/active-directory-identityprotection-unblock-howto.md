---
title: "aaaAzure Active Directory Identity Protection - jak uživatelé toounblock | Microsoft Docs"
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
ms.openlocfilehash: cdda2808822888f76aa75cf46478738c94df51a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection---how-toounblock-users"></a><span data-ttu-id="cc3f9-104">Azure Active Directory identitu ochrana – jak toounblock uživatelů</span><span class="sxs-lookup"><span data-stu-id="cc3f9-104">Azure Active Directory Identity Protection - How toounblock users</span></span>
<span data-ttu-id="cc3f9-105">S Azure Active Directory Identity Protection můžete nakonfigurovat zásady, které uživatelé tooblock Pokud hello nakonfigurované podmínky jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-105">With Azure Active Directory Identity Protection, you can configure policies tooblock users if hello configured conditions are satisfied.</span></span> <span data-ttu-id="cc3f9-106">Obvykle blokovaný uživatel kontakty nápovědy podpory toobecome odblokováno.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-106">Typically, a blocked user contacts help desk toobecome unblocked.</span></span> <span data-ttu-id="cc3f9-107">Tato témata vysvětluje hello kroky můžete provádět toounblock blokovaný uživatel.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-107">This topics explains hello steps you can perform toounblock a blocked user.</span></span>

## <a name="determine-hello-reason-for-blocking"></a><span data-ttu-id="cc3f9-108">Určení hello důvod pro blokování</span><span class="sxs-lookup"><span data-stu-id="cc3f9-108">Determine hello reason for blocking</span></span>
<span data-ttu-id="cc3f9-109">Jako první krok toounblock uživatele musíte toodetermine hello typ zásad, který má blokovaný hello uživatele, protože na něm závisí další kroky.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-109">As a first step toounblock a user, you need toodetermine hello type of policy that has blocked hello user because your next steps are depending on it.</span></span>
<span data-ttu-id="cc3f9-110">S Azure Active Directory Identity Protection může se uživatel buď blokovaný přihlášení riziko zásadu nebo zásady riziko uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-110">With Azure Active Directory Identity Protection, a user can be either blocked by a sign-in risk policy or a user risk policy.</span></span>

<span data-ttu-id="cc3f9-111">Můžete získat hello typ zásad, který má blokovaný uživatel z hello záhlaví v dialogovém okně hello, který byl předložený toohello uživatele při pokusu o přihlášení:</span><span class="sxs-lookup"><span data-stu-id="cc3f9-111">You can get hello type of policy that has blocked a user from hello heading in hello dialog that was presented toohello user during a sign-in attempt:</span></span>

| <span data-ttu-id="cc3f9-112">Zásada</span><span class="sxs-lookup"><span data-stu-id="cc3f9-112">Policy</span></span> | <span data-ttu-id="cc3f9-113">Dialogové okno uživatele</span><span class="sxs-lookup"><span data-stu-id="cc3f9-113">User dialog</span></span> |
| --- | --- |
| <span data-ttu-id="cc3f9-114">Riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="cc3f9-114">Sign-in risk</span></span> |![Blokované přihlášení](./media/active-directory-identityprotection-unblock-howto/02.png) |
| <span data-ttu-id="cc3f9-116">Riziko pro uživatele</span><span class="sxs-lookup"><span data-stu-id="cc3f9-116">User risk</span></span> |![Zablokovaný účet](./media/active-directory-identityprotection-unblock-howto/104.png) |

<span data-ttu-id="cc3f9-118">Uživatel, který je blokována:</span><span class="sxs-lookup"><span data-stu-id="cc3f9-118">A user that is blocked by:</span></span>

* <span data-ttu-id="cc3f9-119">Zásady přihlášení riziko je také označován jako podezřelou přihlášení</span><span class="sxs-lookup"><span data-stu-id="cc3f9-119">A sign-in risk policy is also known as suspicious sign-in</span></span>
* <span data-ttu-id="cc3f9-120">Zásady uživatele riziko je také označován jako účet v ohrožení</span><span class="sxs-lookup"><span data-stu-id="cc3f9-120">A user risk policy is also known as an account at risk</span></span>

## <a name="unblocking-suspicious-sign-ins"></a><span data-ttu-id="cc3f9-121">Odblokování podezřelé přihlášení</span><span class="sxs-lookup"><span data-stu-id="cc3f9-121">Unblocking suspicious sign-ins</span></span>
<span data-ttu-id="cc3f9-122">toounblock podezřelé přihlášení, máte následující možnosti hello:</span><span class="sxs-lookup"><span data-stu-id="cc3f9-122">toounblock a suspicious sign-in, you have hello following options:</span></span>

1. <span data-ttu-id="cc3f9-123">**Přihlášení ze zařízení nebo známé umístění** -běžným důvodem pro blokované podezřelé přihlášení jsou pokusů o přihlášení z neznámých míst nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-123">**Sign-in from a familiar location or device** - A common reason for blocked suspicious sign-ins are sign-in attempts from unfamiliar locations or devices.</span></span> <span data-ttu-id="cc3f9-124">Vaši uživatelé můžete rychle zjistit, zda je hello Důvod blokování tak, že zkusíte toosign-in ze známé umístění nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-124">Your users can quickly determine whether this is hello blocking reason by trying toosign-in from a familiar location or device.</span></span>
2. <span data-ttu-id="cc3f9-125">**Vyloučit ze zásad** – Pokud se domníváte, že hello aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit hello uživatelé z něj.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-125">**Exclude from policy** - If you think that hello current configuration of your sign-in policy is causing issues for specific users, you can exclude hello users from it.</span></span> <span data-ttu-id="cc3f9-126">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-126">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
3. <span data-ttu-id="cc3f9-127">**Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat hello zásad.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-127">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable hello policy.</span></span> <span data-ttu-id="cc3f9-128">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-128">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="unblocking-accounts-at-risk"></a><span data-ttu-id="cc3f9-129">Odblokování účty v ohrožení</span><span class="sxs-lookup"><span data-stu-id="cc3f9-129">Unblocking accounts at risk</span></span>
<span data-ttu-id="cc3f9-130">toounblock na účtu ohrožený, máte následující možnosti hello:</span><span class="sxs-lookup"><span data-stu-id="cc3f9-130">toounblock an account at risk, you have hello following options:</span></span>

1. <span data-ttu-id="cc3f9-131">**Resetovat heslo** -resetujete heslo uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-131">**Reset password** - You can reset hello user's password.</span></span> <span data-ttu-id="cc3f9-132">V tématu [ruční zabezpečené heslo resetovat](active-directory-identityprotection.md#manual-secure-password-reset) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-132">See [manual secure password reset](active-directory-identityprotection.md#manual-secure-password-reset) for more details.</span></span>
2. <span data-ttu-id="cc3f9-133">**Zavřít všechny události riziko** – Pokud bylo dosaženo úroveň rizika uživatele hello nakonfigurované pro blokování přístupu blokuje hello uživatele riziko zásad uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-133">**Dismiss all risk events** - hello user risk policy blocks a user if hello configured user risk level for blocking access has been reached.</span></span> <span data-ttu-id="cc3f9-134">Můžete snížit uživatele je úroveň rizika ručně ukončením hlášené rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-134">You can reduce a user's risk level by manually closing reported risk events.</span></span> <span data-ttu-id="cc3f9-135">Další podrobnosti najdete v tématu [zavření rizikových událostí ručně](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="cc3f9-135">For more details, see [closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>
3. <span data-ttu-id="cc3f9-136">**Vyloučit ze zásad** – Pokud se domníváte, že hello aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit hello uživatelé z něj.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-136">**Exclude from policy** - If you think that hello current configuration of your sign-in policy is causing issues for specific users, you can exclude hello users from it.</span></span> <span data-ttu-id="cc3f9-137">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-137">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>
4. <span data-ttu-id="cc3f9-138">**Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat hello zásad.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-138">**Disable policy** - If you think that your policy configuration is causing issues for all your users, you can disable hello policy.</span></span> <span data-ttu-id="cc3f9-139">V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cc3f9-139">See [Azure Active Directory Identity Protection](active-directory-identityprotection.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc3f9-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc3f9-140">Next steps</span></span>
 <span data-ttu-id="cc3f9-141">Chcete, aby tooknow Další informace o Azure AD Identity Protection?</span><span class="sxs-lookup"><span data-stu-id="cc3f9-141">Do you want tooknow more about Azure AD Identity Protection?</span></span> <span data-ttu-id="cc3f9-142">Podívejte se na [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="cc3f9-142">Check out [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>
