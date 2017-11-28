---
title: "aaaHow toocomplete kontrola přístupu | Microsoft Docs"
description: "Po spuštění kontrola přístupu v Azure AD Privileged Identity Management zjistěte, jak toocomplete ho a zobrazení výsledků hello"
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
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="370e9-103">Jak toocomplete přístupu zkontrolujte v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="370e9-103">How toocomplete an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="370e9-104">Správce privilegovaných rolí můžete zkontrolovat jednou privilegovaného přístupu [revize zabezpečení byla spuštěna](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="370e9-104">Privileged role administrators can review privileged access once a [security review has been started](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span> <span data-ttu-id="370e9-105">Azure AD Privileged Identity Management (PIM), bude automaticky posílat výzvy uživatelé tooreview jejich přístup k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="370e9-105">Azure AD Privileged Identity Management (PIM) will automatically send an email prompting users tooreview their access.</span></span> <span data-ttu-id="370e9-106">Pokud uživatel neobdrželi e-mailu, můžete odeslat je hello pokyny [jak tooperform zabezpečení zkontrolujte](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="370e9-106">If a user did not get an email, you can send them hello instructions in [how tooperform a security review](active-directory-privileged-identity-management-how-to-perform-security-review.md).</span></span>

<span data-ttu-id="370e9-107">Po dobu kontrola zabezpečení hello je nad nebo všichni uživatelé hello dokončení jejich samoobslužné zkontrolujte, postupujte podle kroků hello v této kontrole hello toomanage článek a zobrazit výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-107">After hello security review period is over, or all hello users have finished their self-review, follow hello steps in this article toomanage hello review and see hello results.</span></span>

## <a name="manage-security-reviews"></a><span data-ttu-id="370e9-108">Správa zabezpečení recenze</span><span class="sxs-lookup"><span data-stu-id="370e9-108">Manage security reviews</span></span>
1. <span data-ttu-id="370e9-109">Přejděte toohello [portál Azure](https://portal.azure.com/) a vyberte hello **Azure AD Privileged Identity Management** aplikace na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="370e9-109">Go toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** application on your dashboard.</span></span>
2. <span data-ttu-id="370e9-110">Vyberte hello **přístup recenze** část řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-110">Select hello **Access reviews** section of hello dashboard.</span></span>
3. <span data-ttu-id="370e9-111">Vyberte, které chcete toomanage kontrola přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-111">Select hello access review that you want toomanage.</span></span>

<span data-ttu-id="370e9-112">V okně podrobností kontrola přístupu hello jsou číslo možností pro správu této revize.</span><span class="sxs-lookup"><span data-stu-id="370e9-112">On hello access review's detail blade there are a number options for managing that review.</span></span>

![PIM přístupu zkontrolujte tlačítka – snímek obrazovky][1]

### <a name="remind"></a><span data-ttu-id="370e9-114">Připomenout</span><span class="sxs-lookup"><span data-stu-id="370e9-114">Remind</span></span>
<span data-ttu-id="370e9-115">Pokud kontrola přístupu je nastaven tak, aby uživatelé hello zkontrolujte sami, hello **připomenutí** tlačítko odesílá oznámení.</span><span class="sxs-lookup"><span data-stu-id="370e9-115">If an access review is set up so that hello users review themselves, hello **Remind** button sends out a notification.</span></span> 

### <a name="stop"></a><span data-ttu-id="370e9-116">Zastavit</span><span class="sxs-lookup"><span data-stu-id="370e9-116">Stop</span></span>
<span data-ttu-id="370e9-117">Všechny recenze přístup mít koncové datum, ale můžete použít hello **Zastavit** tlačítko toofinish je již v rané fázi.</span><span class="sxs-lookup"><span data-stu-id="370e9-117">All access reviews have an end date, but you can use hello **Stop** button toofinish it early.</span></span> <span data-ttu-id="370e9-118">Pokud žádné uživatele nebyly revidován té doby, nebudou moct tooafter zastavíte zkontrolujte hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-118">If any users haven't been reviewed by this time, they won't be able tooafter you stop hello review.</span></span> <span data-ttu-id="370e9-119">Kontrolu nelze restartovat po byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="370e9-119">You cannot restart a review after it's been stopped.</span></span>

### <a name="apply"></a><span data-ttu-id="370e9-120">Použít</span><span class="sxs-lookup"><span data-stu-id="370e9-120">Apply</span></span>
<span data-ttu-id="370e9-121">Po dokončení kontrola přístupu buď protože bylo dosaženo maximálního hello koncové datum nebo byla ručně zastavena hello **použít** tlačítko implementuje hello výsledek zkontrolujte hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-121">After an access review is completed, either because you reached hello end date or stopped it manually, hello **Apply** button implements hello outcome of hello review.</span></span> <span data-ttu-id="370e9-122">Pokud v hello zkontrolujte byl odepřen přístup uživatele, je to hello krok, který odstraní přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="370e9-122">If a user's access was denied in hello review, this is hello step that will remove their role assignment.</span></span>  

### <a name="export"></a><span data-ttu-id="370e9-123">Export</span><span class="sxs-lookup"><span data-stu-id="370e9-123">Export</span></span>
<span data-ttu-id="370e9-124">Pokud chcete výsledky hello tooapply revize zabezpečení hello ručně, můžete exportovat zkontrolujte hello.</span><span class="sxs-lookup"><span data-stu-id="370e9-124">If you want tooapply hello results of hello security review manually, you can export hello review.</span></span> <span data-ttu-id="370e9-125">Hello **exportovat** tlačítko se spustí stahování souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="370e9-125">hello **Export** button will start downloading a CSV file.</span></span> <span data-ttu-id="370e9-126">Můžete spravovat hello výsledky v aplikaci Excel nebo programy, které soubory sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="370e9-126">You can manage hello results in Excel or other programs that open CSV files.</span></span>

### <a name="delete"></a><span data-ttu-id="370e9-127">Odstranění</span><span class="sxs-lookup"><span data-stu-id="370e9-127">Delete</span></span>
<span data-ttu-id="370e9-128">Pokud si nejste zájem o hello žádné další zkontrolovat, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="370e9-128">If you are not interested in hello review any further, delete it.</span></span> <span data-ttu-id="370e9-129">Hello **odstranit** tlačítko odebere kontrolní hello hello PIM aplikace.</span><span class="sxs-lookup"><span data-stu-id="370e9-129">hello **Delete** button removes hello review from hello PIM application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="370e9-130">Nebude zobrazí upozornění, než dojde k odstranění, proto se ujistěte, že chcete toodelete, zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="370e9-130">You will not get a warning before deletion occurs, so be sure that you want toodelete that review.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="370e9-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="370e9-131">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
