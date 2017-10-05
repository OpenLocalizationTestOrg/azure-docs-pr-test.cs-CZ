---
title: "Jak provést kontrola přístupu | Microsoft Docs"
description: "Po spuštění kontrola přístupu v Azure AD Privileged Identity Management zjistěte, jak dokončit ho a zobrazení výsledků"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: ca2a1c7c287e4cf6b1b50cfb0068861b6f525596
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="592b5-103">Jak provést kontrola přístupu v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="592b5-103">How to complete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="592b5-104">Správce privilegovaných rolí můžete zkontrolovat jednou privilegovaného přístupu [revize zabezpečení byla spuštěna](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="592b5-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="592b5-105">Azure AD Privileged Identity Management (PIM), bude automaticky posílat vyzývá uživatele, aby kontrolovat jejich přístup k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="592b5-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users to review their access.</span></span> <span data-ttu-id="592b5-106">Pokud uživatel neobdrželi e-mailu, můžete odeslat je podle pokynů [jak provádět kontrolu zabezpečení](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="592b5-106">If a user did not get an email, you can send them the instructions in [how to perform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="592b5-107">Po dobu kontrola zabezpečení je nad nebo dokončení jejich samoobslužné zkontrolujte všechny uživatele, postupujte podle kroků v tomto článku ke správě kontrola a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="592b5-107">After the security review period is over, or all the users have finished their self-review, follow the steps in this article to manage the review and see the results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="592b5-108">Správa zabezpečení recenze</span><span class="sxs-lookup"><span data-stu-id="592b5-108">Manage security reviews</span></span>
1. <span data-ttu-id="592b5-109">Přejděte na [portál Azure](https://portal.azure.com/) a vyberte **Azure AD Privileged Identity Management** aplikace na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="592b5-109">Go to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="592b5-110">Vyberte **přístup recenze** část řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="592b5-110">Select the **Access reviews** section of the dashboard.</span></span>
3. <span data-ttu-id="592b5-111">Vyberte kontrola přístupu, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="592b5-111">Select the access review that you want to manage.</span></span>

<span data-ttu-id="592b5-112">V okně podrobností kontrola přístupu jsou číslo možností pro správu této revize.</span><span class="sxs-lookup"><span data-stu-id="592b5-112">On the access review's detail blade there are a number options for managing that review.</span></span>

![PIM přístupu zkontrolujte tlačítka – snímek obrazovky][1]

### <a name="remind"></a><span data-ttu-id="592b5-114">Připomenout</span><span class="sxs-lookup"><span data-stu-id="592b5-114">Remind</span></span>
<span data-ttu-id="592b5-115">Pokud je kontrola přístupu nastavený tak, aby uživatelé sami, zkontrolujte **připomenutí** tlačítko odesílá oznámení.</span><span class="sxs-lookup"><span data-stu-id="592b5-115">If an access review is set up so that the users review themselves, the **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="592b5-116">Zastavit</span><span class="sxs-lookup"><span data-stu-id="592b5-116">Stop</span></span>
<span data-ttu-id="592b5-117">Všechny recenze přístup mít koncové datum, ale můžete použít **Zastavit** tlačítko ukončíte již v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="592b5-117">All access reviews have an end date, but you can use the **Stop** button to finish it early.</span></span> <span data-ttu-id="592b5-118">Pokud žádné uživatele nebyly revidován té doby, nebudou moci po ukončení kontrola.</span><span class="sxs-lookup"><span data-stu-id="592b5-118">If any users haven't been reviewed by this time, they won't be able to after you stop the review.</span></span> <span data-ttu-id="592b5-119">Kontrolu nelze restartovat po byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="592b5-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="592b5-120">Použít</span><span class="sxs-lookup"><span data-stu-id="592b5-120">Apply</span></span>
<span data-ttu-id="592b5-121">Po dokončení kontrola přístupu buď protože bylo dosaženo maximálního koncové datum nebo byla ručně zastavena **použít** tlačítko implementuje výsledek kontrola.</span><span class="sxs-lookup"><span data-stu-id="592b5-121">After an access review is completed, either because you reached the end date or stopped it manually, the **Apply** button implements the outcome of the review.</span></span> <span data-ttu-id="592b5-122">Pokud v kontrola byl odepřen přístup uživatele, je to krok, který odstraní přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="592b5-122">If a user's access was denied in the review, this is the step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="592b5-123">Export</span><span class="sxs-lookup"><span data-stu-id="592b5-123">Export</span></span>
<span data-ttu-id="592b5-124">Pokud chcete použít výsledků revize zabezpečení ručně, můžete exportovat kontrola.</span><span class="sxs-lookup"><span data-stu-id="592b5-124">If you want to apply the results of the security review manually, you can export the review.</span></span> <span data-ttu-id="592b5-125">**Exportovat** tlačítko se spustí stahování souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="592b5-125">The **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="592b5-126">Můžete spravovat výsledky v aplikaci Excel nebo programy, které soubory sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="592b5-126">You can manage the results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="592b5-127">Odstranění</span><span class="sxs-lookup"><span data-stu-id="592b5-127">Delete</span></span>
<span data-ttu-id="592b5-128">Pokud si nejste zájem o žádné další kontrolní, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="592b5-128">If you are not interested in the review any further, delete it.</span></span> <span data-ttu-id="592b5-129">**Odstranit** tlačítko odstraní kontrola z aplikace PIM.</span><span class="sxs-lookup"><span data-stu-id="592b5-129">The **Delete** button removes the review from the PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="592b5-130">Nebude zobrazí upozornění, než dojde k odstranění, proto se ujistěte, že chcete odstranit tento kontrolní.</span><span class="sxs-lookup"><span data-stu-id="592b5-130">You will not get a warning before deletion occurs, so be sure that you want to delete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="592b5-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="592b5-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
