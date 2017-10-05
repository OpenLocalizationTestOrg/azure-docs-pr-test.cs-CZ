---
title: "Jak poskytnout přístup k Privileged Identity Management - Azure | Microsoft Docs"
description: "Informace o postupu přidání role pro uživatele s příponou Azure Active Directory Privileged Identity Management, tak můžou spravovat PIM."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: aeaefb484b29da6e89c2c3c650a79a881b3fa5b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a><span data-ttu-id="49d21-103">Udělení přístupu ke správě Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="49d21-103">Giving access to manage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="49d21-104">Globální správce, který umožňuje Azure AD Privileged Identity Management (PIM) pro organizaci automaticky získat přiřazení rolí a přístup k PIM.</span><span class="sxs-lookup"><span data-stu-id="49d21-104">The global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access to PIM.</span></span> <span data-ttu-id="49d21-105">Nikdo jiný získá přístup pro zápis ve výchozím nastavení, ale včetně další globální správce.</span><span class="sxs-lookup"><span data-stu-id="49d21-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="49d21-106">Další globální správci, správce zabezpečení a zabezpečení čtečky mají přístup jen pro čtení k Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="49d21-106">Other global adminstrators, security administrators, and security readers have read-only access to Azure AD PIM.</span></span> <span data-ttu-id="49d21-107">Udělit přístup k PIM, můžete přiřadit první uživatel ostatním uživatelům **správce privilegovaných rolí** role.</span><span class="sxs-lookup"><span data-stu-id="49d21-107">To give access to PIM, the first user can assign others to the **Privileged role administrator** role.</span></span> <span data-ttu-id="49d21-108">Toto přiřazení musí provádět v PIM sám a nelze změnit pomocí prostředí PowerShell nebo dalších portálech.</span><span class="sxs-lookup"><span data-stu-id="49d21-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="49d21-109">Správa Azure AD PIM vyžaduje Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="49d21-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="49d21-110">Vzhledem k tomu, že účty Microsoft nelze zaregistrovat pro Azure MFA, uživatel, který se přihlásí pomocí účtu Microsoft přístup k Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="49d21-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="49d21-111">Zkontrolujte, že jsou vždy alespoň dva uživatelé v roli správce privilegovaných rolí v případě, že jeden uživatel je uzamčen nebo jejich účet je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="49d21-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-to-manage-pim"></a><span data-ttu-id="49d21-112">Zadejte jiný uživatel přístup ke správě PIM</span><span class="sxs-lookup"><span data-stu-id="49d21-112">Give another user access to manage PIM</span></span>
1. <span data-ttu-id="49d21-113">Přihlaste se k [portál Azure](https://portal.azure.com/) a vyberte **Azure AD Privileged Identity Management** aplikace na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="49d21-113">Sign in to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** app on the dashboard.</span></span>
2. <span data-ttu-id="49d21-114">Vyberte **spravovat privilegované role** > **správce privilegovaných rolí** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="49d21-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Přidání privilegované role správců – snímek obrazovky][1]
3. <span data-ttu-id="49d21-116">V okně Přidat spravované uživatele je krok 1 již dokončena.</span><span class="sxs-lookup"><span data-stu-id="49d21-116">On the Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="49d21-117">Vyberte krok 2, **vyberte uživatele,** a vyhledejte uživatele, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="49d21-117">Select step 2, **Select users** and search for the user you want to add.</span></span>
   
    ![Vyberte uživatele – snímek obrazovky][2]
4. <span data-ttu-id="49d21-119">Ve výsledcích hledání vyberte uživatele a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="49d21-119">Select the user from the search results, and click **Done**.</span></span>
5. <span data-ttu-id="49d21-120">Klikněte na tlačítko **OK** uložte svůj výběr.</span><span class="sxs-lookup"><span data-stu-id="49d21-120">Click **OK** to save your selection.</span></span> <span data-ttu-id="49d21-121">Uživatel, který jste vybrali, se zobrazí v seznamu privilegované role správců.</span><span class="sxs-lookup"><span data-stu-id="49d21-121">The user you have selected will appear in the list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="49d21-122">Vždy, když přiřadíte novou roli uživatele, budou se automaticky nastavit jako způsobilých při aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="49d21-122">Whenever you assign a new role to someone, they are automatically set up as eligible to activate the role.</span></span> <span data-ttu-id="49d21-123">Pokud chcete převést na trvalé v roli, klikněte na uživatele v seznamu.</span><span class="sxs-lookup"><span data-stu-id="49d21-123">If you want to make them permanent in the role, click the user in the list.</span></span> <span data-ttu-id="49d21-124">Vyberte **zkontrolujte oprávnění** v nabídce uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="49d21-124">Select **make perm** in the user information menu.</span></span>
6. <span data-ttu-id="49d21-125">Odkaz na uživateli odeslat [Začínáme s Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="49d21-125">Send the user a link to [Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="49d21-126">Odeberte jiné uživatelská přístupová práva pro správu PIM</span><span class="sxs-lookup"><span data-stu-id="49d21-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="49d21-127">Před odebráním někdo z role správce privilegovaných rolí, vždy ujistěte se, že stále ještě dva uživatelé přiřazení k němu.</span><span class="sxs-lookup"><span data-stu-id="49d21-127">Before you remove someone from the privileged role administrator role, always make sure there will still be two users assigned to it.</span></span>

1. <span data-ttu-id="49d21-128">Na řídicím panelu PIM klikněte na roli **správce privilegovaných rolí**.</span><span class="sxs-lookup"><span data-stu-id="49d21-128">In the PIM dashboard, click on the role **Privileged role administrator**.</span></span>  <span data-ttu-id="49d21-129">Zobrazí se seznam uživatelů aktuálně v dané roli.</span><span class="sxs-lookup"><span data-stu-id="49d21-129">The list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="49d21-130">Klikněte na uživatele v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="49d21-130">Click on the user in the user list.</span></span>
3. <span data-ttu-id="49d21-131">Klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="49d21-131">Click on **Remove**.</span></span>  <span data-ttu-id="49d21-132">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="49d21-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="49d21-133">Klikněte na tlačítko **Ano** odebrat uživatele z role.</span><span class="sxs-lookup"><span data-stu-id="49d21-133">Click **Yes** to remove the user from the role.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="49d21-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49d21-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
