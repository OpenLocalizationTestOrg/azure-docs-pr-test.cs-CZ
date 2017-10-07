---
title: "aaaHow toostart kontrola přístupu | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupu zkontrolujte pro privilegované identity pomocí hello aplikací Azure Privileged Identity Management."
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
ms.openlocfilehash: 24feac307f77c69b5d68d6ae0623dbcb52416b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostart-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="5fa59-103">Jak toostart přístupu zkontrolujte v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="5fa59-103">How toostart an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="5fa59-104">Přiřazení rolí stát "zastaralé", když uživatelé mají privilegovaný přístup, které už nepotřebují.</span><span class="sxs-lookup"><span data-stu-id="5fa59-104">Role assignments become "stale" when users have privileged access that they don't need anymore.</span></span> <span data-ttu-id="5fa59-105">V pořadí tooreduce hello riziko spojené s přiřazení těchto zastaralých rolí Správci privilegované role by pravidelně zkontrolovat hello rolí, které mají uživatelé.</span><span class="sxs-lookup"><span data-stu-id="5fa59-105">In order tooreduce hello risk associated with these stale role assignments, privileged role administrators should regularly review hello roles that users have been given.</span></span> <span data-ttu-id="5fa59-106">Tento dokument popisuje hello kroky pro spuštění kontrola přístupu v Azure AD Privileged Identity Management (PIM).</span><span class="sxs-lookup"><span data-stu-id="5fa59-106">This document covers hello steps for starting an access review in Azure AD Privileged Identity Management (PIM).</span></span>

## <a name="start-an-access-review"></a><span data-ttu-id="5fa59-107">Spuštění kontrola přístupu</span><span class="sxs-lookup"><span data-stu-id="5fa59-107">Start an access review</span></span>
> [!NOTE]
> <span data-ttu-id="5fa59-108">Pokud jste nepřidali řídicí panel tooyour hello PIM aplikací v hello portálu Azure, najdete v části hello kroky v [Začínáme s Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="5fa59-108">If you haven't added hello PIM application tooyour dashboard in hello Azure portal, see hello steps in  [Getting Started with Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)</span></span>
> 
> 

<span data-ttu-id="5fa59-109">Z hello PIM aplikace hlavní stránky existují tři způsoby toostart kontrola přístupu:</span><span class="sxs-lookup"><span data-stu-id="5fa59-109">From hello PIM application main page, there are three ways toostart an access review:</span></span>

* <span data-ttu-id="5fa59-110">**Přístup k recenze** > **přidat**</span><span class="sxs-lookup"><span data-stu-id="5fa59-110">**Access reviews** > **Add**</span></span>
* <span data-ttu-id="5fa59-111">**Role** > **zkontrolujte** tlačítko</span><span class="sxs-lookup"><span data-stu-id="5fa59-111">**Roles** > **Review** button</span></span>
* <span data-ttu-id="5fa59-112">Vyberte hello určité role toobe zkontrolováni z seznam rolí hello > **zkontrolujte** tlačítko</span><span class="sxs-lookup"><span data-stu-id="5fa59-112">Select hello specific role toobe reviewed from hello roles list > **Review** button</span></span>

<span data-ttu-id="5fa59-113">Po kliknutí na hello **zkontrolujte** tlačítko hello **spustit kontrola přístupu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="5fa59-113">When you click on hello **Review** button, hello **Start an access review** blade appears.</span></span> <span data-ttu-id="5fa59-114">V tomto okně jste probíhající tooconfigure hello zkontrolujte s názvem a čas omezení, zvolte roli tooreview a rozhodnout, který provede zkontrolujte hello.</span><span class="sxs-lookup"><span data-stu-id="5fa59-114">On this blade, you're going tooconfigure hello review with a name and time limit, choose a role tooreview, and decide who will perform hello review.</span></span>

![Kontrola přístupu – snímek obrazovky Start][1]

### <a name="configure-hello-review"></a><span data-ttu-id="5fa59-116">Konfigurace zkontrolujte hello</span><span class="sxs-lookup"><span data-stu-id="5fa59-116">Configure hello review</span></span>
<span data-ttu-id="5fa59-117">Zkontrolujte toocreate přístup, budete potřebovat tooname ho a nastaví počáteční a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="5fa59-117">toocreate an access review, you need tooname it and set a start and end date.</span></span>

![Nakonfigurujte kontrolní – snímek obrazovky][2]

<span data-ttu-id="5fa59-119">Ujistěte se, hello délka hello zkontrolujte dostatečně dlouhou dobu toocomplete uživatele.</span><span class="sxs-lookup"><span data-stu-id="5fa59-119">Make hello length of hello review long enough for users toocomplete it.</span></span> <span data-ttu-id="5fa59-120">Pokud dokončíte před hello koncové datum, můžete vždy zastavit hello zkontrolujte již v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="5fa59-120">If you finish before hello end date, you can always stop hello review early.</span></span>

### <a name="choose-a-role-tooreview"></a><span data-ttu-id="5fa59-121">Vyberte role tooreview</span><span class="sxs-lookup"><span data-stu-id="5fa59-121">Choose a role tooreview</span></span>
<span data-ttu-id="5fa59-122">Každý revize se zaměřuje na jen jednu roli.</span><span class="sxs-lookup"><span data-stu-id="5fa59-122">Each review focuses on only one role.</span></span> <span data-ttu-id="5fa59-123">Pokud jste začali kontrola přístupu hello v okně konkrétní roli, budete potřebovat toochoose roli teď.</span><span class="sxs-lookup"><span data-stu-id="5fa59-123">Unless you started hello access review from a specific role blade, you'll need toochoose a role now.</span></span>

1. <span data-ttu-id="5fa59-124">Přejděte příliš**Zkontrolujte členství v roli**</span><span class="sxs-lookup"><span data-stu-id="5fa59-124">Navigate too**Review role membership**</span></span>
   
    ![Zkontrolujte členství v rolích – snímek obrazovky][3]
2. <span data-ttu-id="5fa59-126">Vyberte jednu roli ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="5fa59-126">Choose one role from hello list.</span></span>

### <a name="decide-who-will-perform-hello-review"></a><span data-ttu-id="5fa59-127">Rozhodněte, který provede zkontrolujte hello</span><span class="sxs-lookup"><span data-stu-id="5fa59-127">Decide who will perform hello review</span></span>
<span data-ttu-id="5fa59-128">Existují tři možnosti pro provádění kontrolu.</span><span class="sxs-lookup"><span data-stu-id="5fa59-128">There are three options for performing a review.</span></span> <span data-ttu-id="5fa59-129">Můžete přiřadit hello zkontrolujte toosomeone else toocomplete, můžete provést sami, nebo může mít každý uživatel, zkontrolujte své vlastní přístup.</span><span class="sxs-lookup"><span data-stu-id="5fa59-129">You can assign hello review toosomeone else toocomplete, you can do it yourself, or you can have each user review their own access.</span></span>

1. <span data-ttu-id="5fa59-130">Přejděte příliš**vyberte**</span><span class="sxs-lookup"><span data-stu-id="5fa59-130">Navigate too**Select reviewers**</span></span>
   
    ![Vyberte – snímek obrazovky][4]
2. <span data-ttu-id="5fa59-132">Vyberte jednu z možností hello:</span><span class="sxs-lookup"><span data-stu-id="5fa59-132">Choose one of hello options:</span></span>
   
   * <span data-ttu-id="5fa59-133">**Vyberte kontrolora**: tuto možnost použijte, pokud si nejste jisti, který potřebuje přístup.</span><span class="sxs-lookup"><span data-stu-id="5fa59-133">**Select reviewer**: Use this option when you don't know who needs access.</span></span> <span data-ttu-id="5fa59-134">Pomocí této možnosti můžete přiřadit vlastníka prostředku tooa zkontrolujte hello nebo toocomplete správce skupiny.</span><span class="sxs-lookup"><span data-stu-id="5fa59-134">With this option, you can assign hello review tooa resource owner or group manager toocomplete.</span></span>
   * <span data-ttu-id="5fa59-135">**Poslat mi**: užitečné, pokud chcete, aby toopreview jak pracovní zkontroluje přístup, nebo chcete tooreview jménem osoby, které nelze.</span><span class="sxs-lookup"><span data-stu-id="5fa59-135">**Me**: Useful if you want toopreview how access reviews work, or you want tooreview on behalf of people who can't.</span></span>
   * <span data-ttu-id="5fa59-136">**Členy zkontrolujte sami**: použití této možnosti toohave hello uživatelé kontrole vlastní přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="5fa59-136">**Members review themselves**: Use this option toohave hello users review their own role assignments.</span></span>

### <a name="start-hello-review"></a><span data-ttu-id="5fa59-137">Spustit kontrolní hello</span><span class="sxs-lookup"><span data-stu-id="5fa59-137">Start hello review</span></span>
<span data-ttu-id="5fa59-138">Nakonec máte toorequire hello možnost, že uživatelé zadat příslušný důvod. Pokud schválí jejich přístup.</span><span class="sxs-lookup"><span data-stu-id="5fa59-138">Finally, you have hello option toorequire that users provide a reason if they approve their access.</span></span> <span data-ttu-id="5fa59-139">Pokud chcete přidat popis hello zkontrolujte a vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="5fa59-139">Add a description of hello review if you like, and select **Start**.</span></span>

<span data-ttu-id="5fa59-140">Zajistěte, aby dát uživatelům vědět, že je kontrola přístupu jim a zobrazit je [jak tooperform přístupu zkontrolujte](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="5fa59-140">Make sure you let your users know that there's an access review waiting for them, and show them [How tooperform an access review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

## <a name="manage-hello-access-review"></a><span data-ttu-id="5fa59-141">Spravovat kontrola přístupu hello</span><span class="sxs-lookup"><span data-stu-id="5fa59-141">Manage hello access review</span></span>
<span data-ttu-id="5fa59-142">Můžete sledovat průběh hello hello kontroloři dokončení jejich recenze v hello řídicí panel Azure AD PIM, v hello zkontroluje přístup k oddílu.</span><span class="sxs-lookup"><span data-stu-id="5fa59-142">You can track hello progress as hello reviewers complete their reviews in hello Azure AD PIM dashboard, in hello access reviews section.</span></span> <span data-ttu-id="5fa59-143">Žádné oprávnění se změní v adresáři hello dokud [dokončení zkontrolujte hello](active-directory-privileged-identity-management-how-to-complete-review.md).</span><span class="sxs-lookup"><span data-stu-id="5fa59-143">No access rights will be changed in hello directory until [hello review completes](active-directory-privileged-identity-management-how-to-complete-review.md).</span></span>

<span data-ttu-id="5fa59-144">Dokud hello zkontrolujte doba je u konce, uživatelé toocomplete můžete připomenout jejich kontrola nebo zastavení hello kontrolní časná z hello přístupu zkontroluje části.</span><span class="sxs-lookup"><span data-stu-id="5fa59-144">Until hello review period is over, you can remind users toocomplete their review, or stop hello review early from hello access reviews section.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="pim-table-of-contents"></a><span data-ttu-id="5fa59-145">Tabulka PIM obsahu</span><span class="sxs-lookup"><span data-stu-id="5fa59-145">PIM Table of Contents</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
