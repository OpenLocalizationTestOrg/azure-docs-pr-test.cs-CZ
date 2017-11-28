---
title: "aaaHow tooperform kontrola přístupu | Microsoft Docs"
description: "Zjistěte, jak hello tooperform a kontrola s aplikací Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="56c58-103">Jak tooperform přístupu zkontrolujte v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="56c58-103">How tooperform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="56c58-104">Azure Active Directory (AD) Privileged Identity Management zjednodušuje, jak spravovat podniky tooresources privilegovaného přístupu ve službě Azure AD a dalším službám Microsoft online jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="56c58-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access tooresources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="56c58-105">Pokud jsou přiřazenou roli správce tooan, privilegovaných rolí správce ve vaší organizaci může požádat tooregularly potvrďte, že je stále nutné této role pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="56c58-105">If you are assigned tooan administrative role, your organization's privileged role administrator may ask you tooregularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="56c58-106">Může získat e-mailu, který obsahuje odkaz, nebo můžete přejít rovnou toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56c58-106">You might get an email that includes a link, or you can go straight toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="56c58-107">Postupujte podle kroků hello v tento článek tooperform samoobslužné zkontrolujte přiřazené role.</span><span class="sxs-lookup"><span data-stu-id="56c58-107">Follow hello steps in this article tooperform a self-review of your assigned roles.</span></span>

<span data-ttu-id="56c58-108">Pokud jste správcem privilegovaných rolí zájem o recenze přístup, získat další podrobnosti o [jak toostart přístupu zkontrolujte](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="56c58-108">If you're a privileged role administrator interested in access reviews, get more details at [How toostart an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-hello-privileged-identity-management-application"></a><span data-ttu-id="56c58-109">Přidání aplikace Privileged Identity Management hello</span><span class="sxs-lookup"><span data-stu-id="56c58-109">Add hello Privileged Identity Management application</span></span>
<span data-ttu-id="56c58-110">Můžete použít aplikaci hello Azure AD Privileged Identity Management (PIM) v hello [portál Azure](https://portal.azure.com/) tooperform zkontrolovali.</span><span class="sxs-lookup"><span data-stu-id="56c58-110">You can use hello Azure AD Privileged Identity Management (PIM) application in hello [Azure portal](https://portal.azure.com/) tooperform your review.</span></span>  <span data-ttu-id="56c58-111">Pokud nemáte hello aplikace Azure AD Privileged Identity Management na portálu, postupujte podle těchto kroků tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="56c58-111">If you don't have hello Azure AD Privileged Identity Management application on your portal, follow these steps tooget started.</span></span>

1. <span data-ttu-id="56c58-112">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="56c58-112">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="56c58-113">Vyberte své uživatelské jméno v hello pravém horním rohu hello portál Azure a vyberte hello adresáře, kde vám budou pracovat.</span><span class="sxs-lookup"><span data-stu-id="56c58-113">Select your username in hello upper right-hand corner of hello Azure portal, and select hello directory where you will you be operating.</span></span>
3. <span data-ttu-id="56c58-114">Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="56c58-114">Select **More services** and use hello Filter textbox toosearch for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="56c58-115">Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="56c58-115">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="56c58-116">Otevře se Hello aplikace Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="56c58-116">hello Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="56c58-117">Schválí nebo zamítne přístup</span><span class="sxs-lookup"><span data-stu-id="56c58-117">Approve or deny access</span></span>
<span data-ttu-id="56c58-118">Při schválit nebo zamítnout přístup, můžete se právě informuje hello kontrolora zda můžete dál používat tuto roli nebo ne.</span><span class="sxs-lookup"><span data-stu-id="56c58-118">When you approve or deny access, you're just telling hello reviewer whether you still use this role or not.</span></span> <span data-ttu-id="56c58-119">Zvolte **schválit** toostay hello role, chcete-li nebo **Odepřít** Pokud jste nebudete potřebovat hello přístup už.</span><span class="sxs-lookup"><span data-stu-id="56c58-119">Choose **Approve** if you want toostay in hello role, or **Deny** if you don't need hello access anymore.</span></span> <span data-ttu-id="56c58-120">Stav vaší nedojde ke změně hned, dokud hello kontrolor platí hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="56c58-120">Your status won't change right away, until hello reviewer applies hello results.</span></span>
<span data-ttu-id="56c58-121">Postupujte podle těchto kroků toofind a dokončete kontrola přístupu hello:</span><span class="sxs-lookup"><span data-stu-id="56c58-121">Follow these steps toofind and complete hello access review:</span></span>

1. <span data-ttu-id="56c58-122">V hello PIM aplikace, vyberte **zkontrolujte privilegovaný přístup**.</span><span class="sxs-lookup"><span data-stu-id="56c58-122">In hello PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="56c58-123">Pokud máte všechny recenze čekající přístup, zobrazí se hello Azure AD přístup kontroluje okno.</span><span class="sxs-lookup"><span data-stu-id="56c58-123">If you have any pending access reviews, they appear in hello Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="56c58-124">Vyberte kontrolní hello chcete toocomplete.</span><span class="sxs-lookup"><span data-stu-id="56c58-124">Select hello review you want toocomplete.</span></span>
3. <span data-ttu-id="56c58-125">Pokud jste vytvořili hello kontrolní, zobrazí jako hello pouze uživatel probíhající kontrola hello.</span><span class="sxs-lookup"><span data-stu-id="56c58-125">Unless you created hello review, you appear as hello only user in hello review.</span></span> <span data-ttu-id="56c58-126">Vyberte název další tooyour hello zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="56c58-126">Select hello check mark next tooyour name.</span></span>
4. <span data-ttu-id="56c58-127">Vyberte buď **schválit** nebo **Odepřít**.</span><span class="sxs-lookup"><span data-stu-id="56c58-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="56c58-128">Může být nutné tooinclude důvod pro vaše rozhodnutí v hello **zadat příslušný důvod** textové pole.</span><span class="sxs-lookup"><span data-stu-id="56c58-128">You may need tooinclude a reason for your decision in hello **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="56c58-129">Zavřít hello **rolí zkontrolujte Azure AD** okno.</span><span class="sxs-lookup"><span data-stu-id="56c58-129">Close hello **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="56c58-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56c58-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
