---
title: "Postup kontrola přístupu | Microsoft Docs"
description: "Zjistěte, jak provést kontrolu s aplikací Azure Privileged Identity Management."
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
ms.openlocfilehash: a98ed60221eeba1d9c92df846aeae2deafb8ae60
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="84e5b-103">Jak provádět kontrola přístupu v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="84e5b-103">How to perform an access review in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="84e5b-104">Azure Active Directory (AD) Privileged Identity Management zjednodušuje, jak spravovat podniky privilegovaný přístup k prostředkům v Azure AD a dalším službám Microsoft online jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="84e5b-104">Azure Active Directory (AD) Privileged Identity Management simplifies how enterprises manage privileged access to resources in Azure AD and other Microsoft online services like Office 365 or Microsoft Intune.</span></span>  

<span data-ttu-id="84e5b-105">Pokud jste přiřazeni k roli správce privilegovaných rolí správce ve vaší organizaci může požádáni, abyste pravidelně potvrďte, že je stále nutné této role pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="84e5b-105">If you are assigned to an administrative role, your organization's privileged role administrator may ask you to regularly confirm that you still need that role for your job.</span></span> <span data-ttu-id="84e5b-106">Může získat e-mailu, který obsahuje odkaz, nebo můžete přejít přímo na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="84e5b-106">You might get an email that includes a link, or you can go straight to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="84e5b-107">Postupujte podle kroků v tomto článku provést samoobslužné zkontrolujte přiřazené role.</span><span class="sxs-lookup"><span data-stu-id="84e5b-107">Follow the steps in this article to perform a self-review of your assigned roles.</span></span>

<span data-ttu-id="84e5b-108">Pokud jste správcem privilegovaných rolí zájem o recenze přístup, získat další podrobnosti o [spuštění kontrola přístupu](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="84e5b-108">If you're a privileged role administrator interested in access reviews, get more details at [How to start an access review](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="add-the-privileged-identity-management-application"></a><span data-ttu-id="84e5b-109">Přidání aplikace Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="84e5b-109">Add the Privileged Identity Management application</span></span>
<span data-ttu-id="84e5b-110">Můžete použít aplikaci Azure AD Privileged Identity Management (PIM) v [portál Azure](https://portal.azure.com/) k provedení zkontrolovali.</span><span class="sxs-lookup"><span data-stu-id="84e5b-110">You can use the Azure AD Privileged Identity Management (PIM) application in the [Azure portal](https://portal.azure.com/) to perform your review.</span></span>  <span data-ttu-id="84e5b-111">Pokud nemáte aplikaci Azure AD Privileged Identity Management na portálu, postupujte podle těchto kroků, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="84e5b-111">If you don't have the Azure AD Privileged Identity Management application on your portal, follow these steps to get started.</span></span>

1. <span data-ttu-id="84e5b-112">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="84e5b-112">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="84e5b-113">Vyberte své uživatelské jméno v pravém horním rohu portálu Azure a vyberte adresář, kde vám budou pracovat.</span><span class="sxs-lookup"><span data-stu-id="84e5b-113">Select your username in the upper right-hand corner of the Azure portal, and select the directory where you will you be operating.</span></span>
3. <span data-ttu-id="84e5b-114">Vyberte **Další služby** a pomocí pole Filtr najděte **Azure AD Privileged Identity Management**.</span><span class="sxs-lookup"><span data-stu-id="84e5b-114">Select **More services** and use the Filter textbox to search for **Azure AD Privileged Identity Management**.</span></span>
4. <span data-ttu-id="84e5b-115">Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="84e5b-115">Check **Pin to dashboard** and then click **Create**.</span></span> <span data-ttu-id="84e5b-116">Aplikace Privileged Identity Management se otevře.</span><span class="sxs-lookup"><span data-stu-id="84e5b-116">The Privileged Identity Management application will open.</span></span>

## <a name="approve-or-deny-access"></a><span data-ttu-id="84e5b-117">Schválí nebo zamítne přístup</span><span class="sxs-lookup"><span data-stu-id="84e5b-117">Approve or deny access</span></span>
<span data-ttu-id="84e5b-118">Při schválit nebo zamítnout přístup, můžete se právě informuje kontrolorovi zda můžete dál používat tuto roli nebo ne.</span><span class="sxs-lookup"><span data-stu-id="84e5b-118">When you approve or deny access, you're just telling the reviewer whether you still use this role or not.</span></span> <span data-ttu-id="84e5b-119">Zvolte **schválit** Pokud chcete, aby zůstali v roli, nebo **Odepřít** Pokud nepotřebujete přístup.</span><span class="sxs-lookup"><span data-stu-id="84e5b-119">Choose **Approve** if you want to stay in the role, or **Deny** if you don't need the access anymore.</span></span> <span data-ttu-id="84e5b-120">Stav vaší nedojde ke změně hned, dokud kontrolorovi platí výsledky.</span><span class="sxs-lookup"><span data-stu-id="84e5b-120">Your status won't change right away, until the reviewer applies the results.</span></span>
<span data-ttu-id="84e5b-121">Postupujte podle těchto kroků můžete najít a dokončit kontrolu přístupu:</span><span class="sxs-lookup"><span data-stu-id="84e5b-121">Follow these steps to find and complete the access review:</span></span>

1. <span data-ttu-id="84e5b-122">V aplikaci PIM vyberte **zkontrolujte privilegovaný přístup**.</span><span class="sxs-lookup"><span data-stu-id="84e5b-122">In the PIM application, select **Review privileged access**.</span></span> <span data-ttu-id="84e5b-123">Pokud máte všechny recenze čekající přístup, zobrazí se v okně recenze přístup k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84e5b-123">If you have any pending access reviews, they appear in the Azure AD Access reviews blade.</span></span>
2. <span data-ttu-id="84e5b-124">Vyberte kontrolní, kterou chcete provést.</span><span class="sxs-lookup"><span data-stu-id="84e5b-124">Select the review you want to complete.</span></span>
3. <span data-ttu-id="84e5b-125">Pokud jste vytvořili kontrola, můžete zobrazit, jako je jediným uživatelem v kontrola.</span><span class="sxs-lookup"><span data-stu-id="84e5b-125">Unless you created the review, you appear as the only user in the review.</span></span> <span data-ttu-id="84e5b-126">Vyberte zaškrtnutí políčka vedle vašeho jména.</span><span class="sxs-lookup"><span data-stu-id="84e5b-126">Select the check mark next to your name.</span></span>
4. <span data-ttu-id="84e5b-127">Vyberte buď **schválit** nebo **Odepřít**.</span><span class="sxs-lookup"><span data-stu-id="84e5b-127">Choose either **Approve** or **Deny**.</span></span> <span data-ttu-id="84e5b-128">Musíte zahrnout důvod pro vaše rozhodnutí v **zadat příslušný důvod** textové pole.</span><span class="sxs-lookup"><span data-stu-id="84e5b-128">You may need to include a reason for your decision in the **Provide a reason** text box.</span></span>  
5. <span data-ttu-id="84e5b-129">Zavřít **rolí zkontrolujte Azure AD** okno.</span><span class="sxs-lookup"><span data-stu-id="84e5b-129">Close the **Review Azure AD roles** blade.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="84e5b-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84e5b-130">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
