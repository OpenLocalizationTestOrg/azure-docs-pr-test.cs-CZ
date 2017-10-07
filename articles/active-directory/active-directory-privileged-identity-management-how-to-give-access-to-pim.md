---
title: "aaaHow toogive přístup tooPrivileged Identity Management - Azure | Microsoft Docs"
description: "Zjistěte, jak tooadd role toousers s hello rozšíření Azure Active Directory Privileged Identity Management, tak můžou spravovat PIM."
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
ms.openlocfilehash: 5d99589af4af766e430d7cecd743ace752f63768
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="giving-access-toomanage-azure-ad-privileged-identity-management"></a><span data-ttu-id="a637e-103">Udělení přístupu toomanage Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="a637e-103">Giving access toomanage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="a637e-104">Hello globální správce, který umožňuje Azure AD Privileged Identity Management (PIM) pro organizaci automaticky získat přiřazení rolí a přístup k tooPIM.</span><span class="sxs-lookup"><span data-stu-id="a637e-104">hello global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access tooPIM.</span></span> <span data-ttu-id="a637e-105">Nikdo jiný získá přístup pro zápis ve výchozím nastavení, ale včetně další globální správce.</span><span class="sxs-lookup"><span data-stu-id="a637e-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="a637e-106">Další globální správci, správce zabezpečení a zabezpečení čtečky mají oprávnění jen pro čtení tooAzure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="a637e-106">Other global adminstrators, security administrators, and security readers have read-only access tooAzure AD PIM.</span></span> <span data-ttu-id="a637e-107">toogive tooPIM přístup, hello prvního uživatele můžete přiřadit jiné toohello **správce privilegovaných rolí** role.</span><span class="sxs-lookup"><span data-stu-id="a637e-107">toogive access tooPIM, hello first user can assign others toohello **Privileged role administrator** role.</span></span> <span data-ttu-id="a637e-108">Toto přiřazení musí provádět v PIM sám a nelze změnit pomocí prostředí PowerShell nebo dalších portálech.</span><span class="sxs-lookup"><span data-stu-id="a637e-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="a637e-109">Správa Azure AD PIM vyžaduje Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="a637e-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="a637e-110">Vzhledem k tomu, že účty Microsoft nelze zaregistrovat pro Azure MFA, uživatel, který se přihlásí pomocí účtu Microsoft přístup k Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="a637e-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="a637e-111">Zkontrolujte, že jsou vždy alespoň dva uživatelé v roli správce privilegovaných rolí v případě, že jeden uživatel je uzamčen nebo jejich účet je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="a637e-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-toomanage-pim"></a><span data-ttu-id="a637e-112">Zadejte jiný uživatel přístup toomanage PIM</span><span class="sxs-lookup"><span data-stu-id="a637e-112">Give another user access toomanage PIM</span></span>
1. <span data-ttu-id="a637e-113">Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte hello **Azure AD Privileged Identity Management** aplikace na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="a637e-113">Sign in toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** app on hello dashboard.</span></span>
2. <span data-ttu-id="a637e-114">Vyberte **spravovat privilegované role** > **správce privilegovaných rolí** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a637e-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Přidání privilegované role správců – snímek obrazovky][1]
3. <span data-ttu-id="a637e-116">V okně spravované uživatele hello přidat je již krok 1 dokončena.</span><span class="sxs-lookup"><span data-stu-id="a637e-116">On hello Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="a637e-117">Vyberte krok 2, **vyberte uživatele,** a vyhledávání pro uživatele hello chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="a637e-117">Select step 2, **Select users** and search for hello user you want tooadd.</span></span>
   
    ![Vyberte uživatele – snímek obrazovky][2]
4. <span data-ttu-id="a637e-119">Vyberte uživatele hello hello výsledky hledání a klikněte na **provádí**.</span><span class="sxs-lookup"><span data-stu-id="a637e-119">Select hello user from hello search results, and click **Done**.</span></span>
5. <span data-ttu-id="a637e-120">Klikněte na tlačítko **OK** toosave váš výběr.</span><span class="sxs-lookup"><span data-stu-id="a637e-120">Click **OK** toosave your selection.</span></span> <span data-ttu-id="a637e-121">Hello uživatele, které jste vybrali, se zobrazí v seznamu hello privilegované role správců.</span><span class="sxs-lookup"><span data-stu-id="a637e-121">hello user you have selected will appear in hello list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="a637e-122">Vždy, když přiřadíte nové toosomeone role, budou se automaticky nastaví jako oprávněné tooactivate hello role.</span><span class="sxs-lookup"><span data-stu-id="a637e-122">Whenever you assign a new role toosomeone, they are automatically set up as eligible tooactivate hello role.</span></span> <span data-ttu-id="a637e-123">Pokud chcete, aby toomake je trvalé hello role, klikněte na tlačítko hello uživatele v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="a637e-123">If you want toomake them permanent in hello role, click hello user in hello list.</span></span> <span data-ttu-id="a637e-124">Vyberte **zkontrolujte oprávnění** v nabídce hello uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="a637e-124">Select **make perm** in hello user information menu.</span></span>
6. <span data-ttu-id="a637e-125">Odeslat hello uživatele odkaz příliš[Začínáme s Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="a637e-125">Send hello user a link too[Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="a637e-126">Odeberte jiné uživatelská přístupová práva pro správu PIM</span><span class="sxs-lookup"><span data-stu-id="a637e-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="a637e-127">Před odebráním někdo z role správce privilegovaných rolí hello, vždy zajistěte, aby stále bude přiřazeno tooit dva uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a637e-127">Before you remove someone from hello privileged role administrator role, always make sure there will still be two users assigned tooit.</span></span>

1. <span data-ttu-id="a637e-128">V hello PIM řídicí panel, klikněte na roli hello **správce privilegovaných rolí**.</span><span class="sxs-lookup"><span data-stu-id="a637e-128">In hello PIM dashboard, click on hello role **Privileged role administrator**.</span></span>  <span data-ttu-id="a637e-129">Zobrazí se seznam Hello uživatelů aktuálně v dané roli.</span><span class="sxs-lookup"><span data-stu-id="a637e-129">hello list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="a637e-130">Klikněte na hello uživatele v seznamu uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="a637e-130">Click on hello user in hello user list.</span></span>
3. <span data-ttu-id="a637e-131">Klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="a637e-131">Click on **Remove**.</span></span>  <span data-ttu-id="a637e-132">Se zobrazí potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="a637e-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="a637e-133">Klikněte na tlačítko **Ano** tooremove hello uživatele z hello role.</span><span class="sxs-lookup"><span data-stu-id="a637e-133">Click **Yes** tooremove hello user from hello role.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="a637e-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a637e-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
