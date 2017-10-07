---
title: "aaaHow tooadd nebo odebrat roli uživatele | Microsoft Docs"
description: "Zjistěte, jak hello tooadd role tooprivileged identity s aplikací Azure Active Directory Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: f84639757dd76061ea12ed6ea7ec9e62ad942109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-privileged-identity-management-how-tooadd-or-remove-a-user-role"></a><span data-ttu-id="ced57-103">Azure AD Privileged Identity Management: Jak tooadd nebo odebrat roli uživatele</span><span class="sxs-lookup"><span data-stu-id="ced57-103">Azure AD Privileged Identity Management: How tooadd or remove a user role</span></span>
<span data-ttu-id="ced57-104">S Azure Active Directory (AD), globální správce (nebo správce společnosti) můžete aktualizovat které uživatelé jsou **trvale** přiřazené tooroles ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ced57-104">With Azure Active Directory (AD), a global administrator (or company administrator) can update which users are **permanently** assigned tooroles in Azure AD.</span></span> <span data-ttu-id="ced57-105">To se provádí pomocí rutin prostředí PowerShell jako `Add-MsolRoleMember` a `Remove-MsolRoleMember`.</span><span class="sxs-lookup"><span data-stu-id="ced57-105">This is done with PowerShell cmdlets like `Add-MsolRoleMember` and `Remove-MsolRoleMember`.</span></span> <span data-ttu-id="ced57-106">Nebo použitím hello portál Azure classic, jak je popsáno v [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ced57-106">Or they can use hello Azure classic portal as described in [assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="ced57-107">Hello aplikace Azure AD Privileged Identity Management umožňuje správci privilegované role toomake přiřazení rolí trvalé, také.</span><span class="sxs-lookup"><span data-stu-id="ced57-107">hello Azure AD Privileged Identity Management application allows privileged role administrators toomake permanent role assignments, as well.</span></span> <span data-ttu-id="ced57-108">Kromě toho privilegované role Správci uživatelé **vhodné** pro role správce.</span><span class="sxs-lookup"><span data-stu-id="ced57-108">Additionally, privileged role administrators can make users **eligible** for admin roles.</span></span> <span data-ttu-id="ced57-109">Oprávněné správce může aktivovat hello role, když je potřebují, a potom jejich oprávnění vyprší po jejich dokončení akce.</span><span class="sxs-lookup"><span data-stu-id="ced57-109">An eligible admin can activate hello role when they need it, and then their permissions expire once they're done.</span></span>

## <a name="manage-roles-with-pim-in-hello-azure-portal"></a><span data-ttu-id="ced57-110">Správa rolí s PIM v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ced57-110">Manage roles with PIM in hello Azure portal</span></span>
<span data-ttu-id="ced57-111">Ve vaší organizaci můžete přiřadit uživatele toodifferent správní role ve službě Azure AD, Office 365 a dalších Microsoft služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="ced57-111">In your organization, you can assign users toodifferent administrative roles in Azure AD, Office 365, and other Microsoft services and applications.</span></span>  <span data-ttu-id="ced57-112">Další informace o dostupných rolí hello najdete na [role v Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ced57-112">More details on hello available roles can be found at [Roles in Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span></span>

<span data-ttu-id="ced57-113">tooadd nebo odebrat uživatele v roli pomocí Privileged Identity Management zprovoznit hello PIM řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="ced57-113">tooadd or remove a user in a role using Privileged Identity Management, bring up hello PIM dashboard.</span></span> <span data-ttu-id="ced57-114">Pak klikněte buď na hello **uživatelé v rolích správce** tlačítko nebo vyberte určité role (jako je například globální správce) z tabulky role hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-114">Then either click hello **Users in Admin Roles** button, or select a specific role (such as Global Administrator) from hello roles table.</span></span>

> [!NOTE]
> <span data-ttu-id="ced57-115">Pokud jste nepovolili PIM v hello portál Azure ještě, přejděte příliš[Začínáme s Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ced57-115">If you haven't enabled PIM in hello Azure portal yet, go too[Get started with Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) for details.</span></span>

<span data-ttu-id="ced57-116">Pokud chcete toogive jiné tooPIM přístupu uživatele, samostatně, hello role, které vyžaduje PIM hello uživatele toohave jsou popsány dále v [přístupu tooPIM toogive](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="ced57-116">If you want toogive another user access tooPIM itself, hello roles which PIM requires hello user toohave are described further in [how toogive access tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="add-a-user-tooa-role"></a><span data-ttu-id="ced57-117">Přidání tooa role uživatele</span><span class="sxs-lookup"><span data-stu-id="ced57-117">Add a user tooa role</span></span>
1. <span data-ttu-id="ced57-118">V hello [portál Azure](https://portal.azure.com/), vyberte hello **Azure AD Privileged Identity Management** dlaždici na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-118">In hello [Azure portal](https://portal.azure.com/), select hello **Azure AD Privileged Identity Management** tile on hello dashboard.</span></span>
2. <span data-ttu-id="ced57-119">Vyberte **spravovat privilegované role**.</span><span class="sxs-lookup"><span data-stu-id="ced57-119">Select **Manage privileged roles**.</span></span>
3. <span data-ttu-id="ced57-120">V hello **Souhrn rolí** tabulky, vyberte hello role, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="ced57-120">In hello **Role summary** table, select hello role you want toomanage.</span></span>
4. <span data-ttu-id="ced57-121">V okně role hello vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ced57-121">In hello role blade, select **Add**.</span></span>
5. <span data-ttu-id="ced57-122">Klikněte na tlačítko **vyberte uživatele,** a vyhledejte uživatele hello na hello **vyberte uživatele,** okno.</span><span class="sxs-lookup"><span data-stu-id="ced57-122">Click **Select users** and search for hello user on hello **Select users** blade.</span></span>  
6. <span data-ttu-id="ced57-123">Vyberte hello uživatele ze seznamu výsledků hledání hello a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="ced57-123">Select hello user from hello search results list, and click **Done**.</span></span>
7. <span data-ttu-id="ced57-124">Klikněte na tlačítko **OK** toosave váš výběr.</span><span class="sxs-lookup"><span data-stu-id="ced57-124">Click **OK** toosave your selection.</span></span> <span data-ttu-id="ced57-125">Hello uživatele, které jste vybrali, se zobrazí v seznamu hello jako vhodné pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-125">hello user you have selected will appear in hello list as eligible for hello role.</span></span>

> [!NOTE]
> <span data-ttu-id="ced57-126">Vhodné pro hello roli ve výchozím nastavení jsou jenom nových uživatelů v roli.</span><span class="sxs-lookup"><span data-stu-id="ced57-126">New users in a role are only eligible for hello role by default.</span></span> <span data-ttu-id="ced57-127">Pokud chcete toomake hello role trvalé, klikněte na tlačítko hello uživatele v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-127">If you want toomake hello role permanent, click hello user in hello list.</span></span> <span data-ttu-id="ced57-128">informace o Hello uživateli se zobrazí v novém okně.</span><span class="sxs-lookup"><span data-stu-id="ced57-128">hello user's information will appear in a new blade.</span></span> <span data-ttu-id="ced57-129">Vyberte **zkontrolujte oprávnění** v nabídce hello uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="ced57-129">Select **Make perm** in hello user information menu.</span></span>  
> <span data-ttu-id="ced57-130">Pokud uživatele nelze zaregistrovat pro Azure Multi-Factor Authentication (MFA), nebo se pomocí účtu Microsoft (obvykle @outlook.com), je nutné toomake je trvalé v jejich rolí.</span><span class="sxs-lookup"><span data-stu-id="ced57-130">If a user cannot register for Azure Multi-Factor Authentication (MFA), or is using a Microsoft account (usually @outlook.com), you need toomake them permanent in all their roles.</span></span> <span data-ttu-id="ced57-131">Oprávněné správce se zobrazí výzva tooregister pro MFA během aktivace.</span><span class="sxs-lookup"><span data-stu-id="ced57-131">Eligible admins are asked tooregister for MFA during activation.</span></span>

<span data-ttu-id="ced57-132">Teď, když hello uživatele je vhodné pro roli, dát jim vědět, že se může aktivovat ho podle pokynů toohello v [jak tooactivate nebo deaktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md).</span><span class="sxs-lookup"><span data-stu-id="ced57-132">Now that hello user is eligible for a role, let them know that they can activate it according toohello instructions in [How tooactivate or deactivate a role](active-directory-privileged-identity-management-how-to-activate-role.md).</span></span>

## <a name="remove-a-user-from-a-role"></a><span data-ttu-id="ced57-133">Odebrat uživatele z role</span><span class="sxs-lookup"><span data-stu-id="ced57-133">Remove a user from a role</span></span>
<span data-ttu-id="ced57-134">Můžete odebrat uživatele z role vhodné přiřazení, ale ujistěte se, že je vždy alespoň jeden uživatel, který je trvalé globálního správce.</span><span class="sxs-lookup"><span data-stu-id="ced57-134">You can remove users from eligible role assignments, but make sure there is always at least one user who is a permanent global administrator.</span></span>

<span data-ttu-id="ced57-135">Postupujte podle těchto kroků tooremove konkrétního uživatele z role:</span><span class="sxs-lookup"><span data-stu-id="ced57-135">Follow these steps tooremove a specific user from a role:</span></span>

1. <span data-ttu-id="ced57-136">Vyberte roli v řídicím panelu Azure AD PIM hello nebo kliknutím na hello přejděte toohello role v seznamu role hello **uživatelé v rolích správce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ced57-136">Navigate toohello role in hello role list either by selecting a role in hello Azure AD PIM dashboard or by clicking on hello **Users in Admin Roles** button.</span></span>
2. <span data-ttu-id="ced57-137">Klikněte na hello uživatele v seznamu uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-137">Click on hello user in hello user list.</span></span>
3. <span data-ttu-id="ced57-138">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="ced57-138">Click **Remove**.</span></span> <span data-ttu-id="ced57-139">Zprávu se zeptá tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ced57-139">A message will ask you tooconfirm.</span></span>
4. <span data-ttu-id="ced57-140">Klikněte na tlačítko **Ano** tooremove hello role od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ced57-140">Click **Yes** tooremove hello role from hello user.</span></span>

<span data-ttu-id="ced57-141">Pokud si nejste jistí, kteří uživatelé stále nutné jejich přiřazení role, pak můžete [spustit kontrola přístupu pro roli hello](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="ced57-141">If you're not sure which users still need their role assignments, then you can [start an access review for hello role](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ced57-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ced57-142">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

