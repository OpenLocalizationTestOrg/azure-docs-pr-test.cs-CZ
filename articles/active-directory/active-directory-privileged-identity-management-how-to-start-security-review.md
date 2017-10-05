---
title: "Postup spuštění kontrola přístupu | Microsoft Docs"
description: "Naučte se vytvářet kontrola přístupu pro privilegované identity s aplikací Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2b516e2f05aa883c5e37f5864e5ee8a2b37d3a46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="4c00f-103">Spuštění kontrola přístupu v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="4c00f-103">How to start an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="4c00f-104">Přiřazení rolí stát "zastaralé", když uživatelé mají privilegovaný přístup, které už nepotřebují.</span><span class="sxs-lookup"><span data-stu-id="4c00f-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="4c00f-105">Chcete-li snížit riziko spojené s přiřazení těchto zastaralých rolí, správci privilegované role by pravidelně zkontrolovat role, které mají uživatelé.</span><span class="sxs-lookup"><span data-stu-id="4c00f-105">In order to reduce the risk associated with these stale role assignments, privileged role administrators should regularly review the roles that users have been given.</span></span> <span data-ttu-id="4c00f-106">Tento dokument popisuje kroky pro spuštění kontrola přístupu v Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="4c00f-106">This document covers the steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="4c00f-107">Spuštění kontrola přístupu</span><span class="sxs-lookup"><span data-stu-id="4c00f-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="4c00f-108">Pokud jste nepřidali aplikaci PIM do řídicího panelu portálu Azure, podívejte se na postup v [Začínáme s Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="4c00f-108">If you haven't added the PIM application to your dashboard in the Azure portal, see the steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="4c00f-109">Na hlavní stránce aplikace PIM existují tři způsoby, jak začít kontrola přístupu:</span><span class="sxs-lookup"><span data-stu-id="4c00f-109">From the PIM application main page, there are three ways to start an access review:</span></span>

* <span data-ttu-id="4c00f-110">**Přístup k recenze** > **přidat**</span><span class="sxs-lookup"><span data-stu-id="4c00f-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="4c00f-111">**Role** > **zkontrolujte** tlačítko</span><span class="sxs-lookup"><span data-stu-id="4c00f-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="4c00f-112">Vyberte konkrétní roli, které mají být zkontrolovány ze seznamu rolí > **zkontrolujte** tlačítko</span><span class="sxs-lookup"><span data-stu-id="4c00f-112">Select the specific role to be reviewed from the roles list > **Review** button</span></span>

<span data-ttu-id="4c00f-113">Po kliknutí na **zkontrolujte** tlačítko **spustit kontrola přístupu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4c00f-113">When you click on the **Review** button, the **Start an access review** blade appears.</span></span> <span data-ttu-id="4c00f-114">V tomto okně budete konfigurovat kontrola s názvem a časový limit, vyberte roli zkontrolovat a rozhodnout, který provede kontrola.</span><span class="sxs-lookup"><span data-stu-id="4c00f-114">On this blade, you're going to configure the review with a name and time limit, choose a role to review, and decide who will perform the review.</span></span>

![Kontrola přístupu – snímek obrazovky Start][1]

### <a name="configure-the-review"></a><span data-ttu-id="4c00f-116">Kontrola konfigurace</span><span class="sxs-lookup"><span data-stu-id="4c00f-116">Configure the review</span></span>
<span data-ttu-id="4c00f-117">Pokud chcete vytvořit kontrola přístupu, potřebujete název a nastavit počáteční a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="4c00f-117">To create an access review, you need to name it and set a start and end date.</span></span>

![Nakonfigurujte kontrolní – snímek obrazovky][2]

<span data-ttu-id="4c00f-119">Ujistěte se, délka dostatečně dlouhé, aby se pro uživatele dokončit, protože se kontrola.</span><span class="sxs-lookup"><span data-stu-id="4c00f-119">Make the length of the review long enough for users to complete it.</span></span> <span data-ttu-id="4c00f-120">Pokud dokončíte před datem ukončení, můžete vždy zastavit kontrola již v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="4c00f-120">If you finish before the end date, you can always stop the review early.</span></span>

### <a name="choose-a-role-to-review"></a><span data-ttu-id="4c00f-121">Vyberte role ke kontrole</span><span class="sxs-lookup"><span data-stu-id="4c00f-121">Choose a role to review</span></span>
<span data-ttu-id="4c00f-122">Každý revize se zaměřuje na jen jednu roli.</span><span class="sxs-lookup"><span data-stu-id="4c00f-122">Each review focuses on only one role.</span></span> <span data-ttu-id="4c00f-123">Pokud jste začali kontrola přístupu v okně konkrétní roli, musíte teď zvolte roli.</span><span class="sxs-lookup"><span data-stu-id="4c00f-123">Unless you started the access review from a specific role blade, you'll need to choose a role now.</span></span>

1. <span data-ttu-id="4c00f-124">Přejděte na **Zkontrolujte členství v roli**</span><span class="sxs-lookup"><span data-stu-id="4c00f-124">Navigate to **Review role membership**</span></span>
   
    ![Zkontrolujte členství v rolích – snímek obrazovky][3]
2. <span data-ttu-id="4c00f-126">Vyberte jednu roli ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4c00f-126">Choose one role from the list.</span></span>

### <a name="decide-who-will-perform-the-review"></a><span data-ttu-id="4c00f-127">Rozhodněte, který provede kontrola</span><span class="sxs-lookup"><span data-stu-id="4c00f-127">Decide who will perform the review</span></span>
<span data-ttu-id="4c00f-128">Existují tři možnosti pro provádění kontrolu.</span><span class="sxs-lookup"><span data-stu-id="4c00f-128">There are three options for performing a review.</span></span> <span data-ttu-id="4c00f-129">Kontrola můžete přiřadit někomu jinému k dokončení, můžete provést sami nebo můžete mít každý uživatel, zkontrolujte své vlastní přístup.</span><span class="sxs-lookup"><span data-stu-id="4c00f-129">You can assign the review to someone else to complete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="4c00f-130">Přejděte do **vyberte**</span><span class="sxs-lookup"><span data-stu-id="4c00f-130">Navigate to **Select reviewers**</span></span>
   
    ![Vyberte – snímek obrazovky][4]
2. <span data-ttu-id="4c00f-132">Vyberte jednu z možností:</span><span class="sxs-lookup"><span data-stu-id="4c00f-132">Choose one of the options:</span></span>
   
   * <span data-ttu-id="4c00f-133">**Vyberte kontrolora**: tuto možnost použijte, pokud si nejste jisti, který potřebuje přístup.</span><span class="sxs-lookup"><span data-stu-id="4c00f-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="4c00f-134">Pomocí této možnosti můžete přiřadit kontrola prostředků vlastníka nebo správce skupiny pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="4c00f-134">With this option, you can assign the review to a resource owner or group manager to complete.</span></span>
   * <span data-ttu-id="4c00f-135">**Poslat mi**: užitečné, pokud si chcete prohlédnout, jak přístupu zkontroluje pracovní, nebo chcete zkontrolovat jménem osoby, které nelze.</span><span class="sxs-lookup"><span data-stu-id="4c00f-135">**Me**: Useful if you want to preview how access reviews work, or you want to review on behalf of people who can't.</span></span>
   * <span data-ttu-id="4c00f-136">**Členy zkontrolujte sami**: tuto možnost použijte, pokud chcete, aby uživatelé Zkontrolujte své vlastní přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="4c00f-136">**Members review themselves**: Use this option to have the users review their own role assignments.</span></span>

### <a name="start-the-review"></a><span data-ttu-id="4c00f-137">Kontrola spuštění</span><span class="sxs-lookup"><span data-stu-id="4c00f-137">Start the review</span></span>
<span data-ttu-id="4c00f-138">Nakonec máte možnost, která vyžaduje, aby uživatelé zadali příslušný důvod. Pokud schválí jejich přístup.</span><span class="sxs-lookup"><span data-stu-id="4c00f-138">Finally, you have the option to require that users provide a reason if they approve their access.</span></span> <span data-ttu-id="4c00f-139">Pokud chcete přidat popis kontrola a vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4c00f-139">Add a description of the review if you like, and select **Start**.</span></span>

<span data-ttu-id="4c00f-140">Zajistěte, aby dát uživatelům vědět, že je kontrola přístupu jim a zobrazit je [postup kontrola přístupu](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="4c00f-140">Make sure you let your users know that there's an access review waiting for them, and show them [How to perform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-the-access-review"></a><span data-ttu-id="4c00f-141">Spravovat kontrola přístupu</span><span class="sxs-lookup"><span data-stu-id="4c00f-141">Manage the access review</span></span>
<span data-ttu-id="4c00f-142">Průběh můžete sledovat, kontroloři dokončení jejich recenze v řídicím panelu Azure AD PIM, v části recenze přístup.</span><span class="sxs-lookup"><span data-stu-id="4c00f-142">You can track the progress as the reviewers complete their reviews in the Azure AD PIM dashboard, in the access reviews section.</span></span> <span data-ttu-id="4c00f-143">Žádné oprávnění se změní v adresáři, dokud [kontrola dokončí](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="4c00f-143">No access rights will be changed in the directory until [the review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="4c00f-144">Dokud zkontrolujte doba je u konce, můžete připomenout uživatelům dokončení jejich kontrola nebo zastavit kontrola již v rané fázi z části recenze přístup.</span><span class="sxs-lookup"><span data-stu-id="4c00f-144">Until the review period is over, you can remind users to complete their review, or stop the review early from the access reviews section.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="4c00f-145">Tabulka PIM obsahu</span><span class="sxs-lookup"><span data-stu-id="4c00f-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
