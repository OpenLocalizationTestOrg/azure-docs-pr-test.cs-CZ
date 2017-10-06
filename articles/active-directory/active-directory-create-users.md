---
title: "aaaAdd tooAzure nového uživatele služby Active Directory | Microsoft Docs"
description: "Vysvětluje, jak tooadd nové uživatele nebo změnit informace o uživateli v Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a><span data-ttu-id="ac363-103">Přidání nových uživatelů nebo uživatelé s tooAzure účty Microsoft, Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac363-103">Add new users or users with Microsoft accounts tooAzure Active Directory</span></span>
<span data-ttu-id="ac363-104">Přidejte uživatele toopopulate adresáře.</span><span class="sxs-lookup"><span data-stu-id="ac363-104">Add users toopopulate your directory.</span></span> <span data-ttu-id="ac363-105">Tento článek vysvětluje, jak tooadd noví uživatelé ve vaší organizaci a jak tooadd uživatelé, kteří mají účty Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ac363-105">This article explains how tooadd new users in your organization, and how tooadd users who have Microsoft accounts.</span></span> <span data-ttu-id="ac363-106">Další informace o přidávání uživatelů z dalších adresářů ve službě Azure Active Directory nebo přidávání uživatelů z partnerských společností najdete v článku o [přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="ac363-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="ac363-107">Přidaní uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete kdykoli přiřadit role toothem.</span><span class="sxs-lookup"><span data-stu-id="ac363-107">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ac363-108">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ac363-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="ac363-109">Pro jak tooadd s uživatelem v Centru pro správu hello Azure AD, najdete v části [přidat nové uživatele tooAzure služby Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac363-109">For how tooadd a user in hello Azure AD admin center, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="ac363-110">Přidání uživatele</span><span class="sxs-lookup"><span data-stu-id="ac363-110">Add a user</span></span>
1. <span data-ttu-id="ac363-111">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="ac363-111">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="ac363-112">Vyberte **služby Active Directory**a potom vyberte hello název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="ac363-112">Select **Active Directory**, and then select hello name of your organization directory.</span></span>
3. <span data-ttu-id="ac363-113">Vyberte hello **uživatelé** a pak v řádku nabídek hello vyberte **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ac363-113">Select hello **Users** tab, and then, in hello command bar, select **Add User**.</span></span>
4. <span data-ttu-id="ac363-114">Na hello **Povězte nám o tohoto uživatele** v části **typ uživatele**, vyberte buď:</span><span class="sxs-lookup"><span data-stu-id="ac363-114">On hello **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="ac363-115">**Nový uživatel v organizaci** – přidá do vašeho adresáře nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="ac363-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="ac363-116">**Uživatel s existujícím účtem Microsoft** – přidá existující Microsoft uživatelský účet tooyour adresář (například účet Outlook)</span><span class="sxs-lookup"><span data-stu-id="ac363-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account tooyour directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="ac363-117">V závislosti na tom, jaký **Typ uživatele** jste vybrali, zadejte buď uživatelské jméno (pro nového uživatele), nebo e-mailovou adresu (pro uživatele s účtem Microsoft).</span><span class="sxs-lookup"><span data-stu-id="ac363-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="ac363-118">Hello uživatele **profil** stránky, zadejte název první a poslední, uživatelské jméno a roli uživatele z hello **role** seznamu.</span><span class="sxs-lookup"><span data-stu-id="ac363-118">On hello user **Profile** page, provide a first and last name, a user-friendly name, and a user role from hello **Roles** list.</span></span> <span data-ttu-id="ac363-119">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ac363-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="ac363-120">Určit, zda příliš**povolit službu Multi-Factor Authentication** pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="ac363-120">Specify whether too**Enable Multi-Factor Authentication** for hello user.</span></span>
7. <span data-ttu-id="ac363-121">Na hello **získat dočasné heslo** vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ac363-121">On hello **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ac363-122">Pokud vaše organizace používá více než jednu doménu, měli byste vědět o hello následující problémy při přidání uživatelského účtu:</span><span class="sxs-lookup"><span data-stu-id="ac363-122">If your organization uses more than one domain, you should know about hello following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="ac363-123">hello tooadd uživatelské účty s totožným hlavním názvem uživatele (UPN) napříč doménami, **první** přidat, například geoffgrisso@contoso.onmicrosoft.com, **následuje** geoffgrisso@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="ac363-123">tooadd user accounts with hello same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="ac363-124">**Nepřidávejte**geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com. Správné pořadí je důležité a může být pracné tooundo.</span><span class="sxs-lookup"><span data-stu-id="ac363-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com. This order is important, and can be cumbersome tooundo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="ac363-125">Změna informací o uživateli</span><span class="sxs-lookup"><span data-stu-id="ac363-125">Change user information</span></span>
<span data-ttu-id="ac363-126">Můžete změnit jakýkoli atribut uživatele s výjimkou ID hello objektu.</span><span class="sxs-lookup"><span data-stu-id="ac363-126">You can change any user attribute except for hello object ID.</span></span>

1. <span data-ttu-id="ac363-127">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="ac363-127">Open your directory.</span></span>
2. <span data-ttu-id="ac363-128">Vyberte hello **uživatelé** kartu a pak vyberte hello zobrazovaný název hello uživatele, bude toochange.</span><span class="sxs-lookup"><span data-stu-id="ac363-128">Select hello **Users** tab, and then select hello display name of hello user you want toochange.</span></span>
3. <span data-ttu-id="ac363-129">Proveďte změny a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ac363-129">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="ac363-130">Pokud hello uživatele, který chcete změnit synchronizována s místní službou Active Directory, nemůžete změnit informace o uživateli hello pomocí tohoto postupu.</span><span class="sxs-lookup"><span data-stu-id="ac363-130">If hello user that you're changing is synchronized with your on-premises Active Directory service, you can't change hello user information using this procedure.</span></span> <span data-ttu-id="ac363-131">toochange hello uživatele, použijte nástroje pro správu vaší místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac363-131">toochange hello user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="ac363-132">Správa a omezení uživatele typu host</span><span class="sxs-lookup"><span data-stu-id="ac363-132">Guest user management and limitations</span></span>
<span data-ttu-id="ac363-133">Účty hostů jsou uživatelé z jiných adresářů, kteří byli pozvané tooyour directory tooaccess SharePoint dokumenty, aplikací nebo jiných prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ac363-133">Guest accounts are users from other directories who were invited tooyour directory tooaccess SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="ac363-134">Účet guest ve vašem adresáři má jeho základní atribut UserType nastavený nastavit příliš "Guest."</span><span class="sxs-lookup"><span data-stu-id="ac363-134">A guest account in your directory has its underlying UserType attribute set too"Guest."</span></span> <span data-ttu-id="ac363-135">Běžní uživatelé (konkrétně členové vašeho adresáře) mají atribut UserType hello "(člen).</span><span class="sxs-lookup"><span data-stu-id="ac363-135">Regular users (specifically, members of your directory) have hello UserType attribute "Member."</span></span>

<span data-ttu-id="ac363-136">Hosté mají omezenou sadu oprávnění v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ac363-136">Guests have a limited set of rights in hello directory.</span></span> <span data-ttu-id="ac363-137">Tato práva omezit možnost hello hosté toodiscover informace o ostatních uživatelích v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ac363-137">These rights limit hello ability for Guests toodiscover information about other users in hello directory.</span></span> <span data-ttu-id="ac363-138">Uživatele typu Host může nadále komunikovat s hello uživatelů a skupin přidružených k hello prostředky, které pracují na.</span><span class="sxs-lookup"><span data-stu-id="ac363-138">However, guest users can still interact with hello users and groups associated with hello resources they're working on.</span></span> <span data-ttu-id="ac363-139">Uživatel typu host může:</span><span class="sxs-lookup"><span data-stu-id="ac363-139">Guest users can:</span></span>

* <span data-ttu-id="ac363-140">Vidět ostatní uživatele a skupiny přidružené toowhich předplatné Azure, který je přiřazený</span><span class="sxs-lookup"><span data-stu-id="ac363-140">See other users and groups associated with an Azure subscription toowhich they're assigned</span></span>
* <span data-ttu-id="ac363-141">V tématu hello členy skupiny toowhich, ke které patří</span><span class="sxs-lookup"><span data-stu-id="ac363-141">See hello members of groups toowhich they belong</span></span>
* <span data-ttu-id="ac363-142">Vyhledávat ostatní uživatele v adresáři hello, v případě, že zná hello úplnou e-mailovou adresu uživatele hello</span><span class="sxs-lookup"><span data-stu-id="ac363-142">Look up other users in hello directory, if they know hello full email address of hello user</span></span>
* <span data-ttu-id="ac363-143">V tématu omezená sada atributů hello uživatelů, které vyhledává – omezené toodisplay jméno, e-mailovou adresu, hlavní název uživatele (UPN) a miniaturu fotografie</span><span class="sxs-lookup"><span data-stu-id="ac363-143">See only a limited set of attributes of hello users they look up--limited toodisplay name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="ac363-144">Získat seznam ověřených domén v adresáři hello</span><span class="sxs-lookup"><span data-stu-id="ac363-144">Get a list of verified domains in hello directory</span></span>
* <span data-ttu-id="ac363-145">Tooapplications souhlasu, je udělení hello stejný přístup, který členové mají ve vašem adresáři</span><span class="sxs-lookup"><span data-stu-id="ac363-145">Consent tooapplications, granting them hello same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="ac363-146">Nastavení zásad přístupu uživatele typu host</span><span class="sxs-lookup"><span data-stu-id="ac363-146">Set guest user access policies</span></span>
<span data-ttu-id="ac363-147">Hello **konfigurace** karta adresáře obsahuje možnosti toocontrol přístupu uživatelů typu Host.</span><span class="sxs-lookup"><span data-stu-id="ac363-147">hello **Configure** tab of a directory includes options toocontrol access for guest users.</span></span> <span data-ttu-id="ac363-148">Tyto možnosti může změnit pouze globální správce adresáře prostřednictvím portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="ac363-148">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="ac363-149">V současné době není k dispozici žádná metoda změny pomocí PowerShellu ani rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ac363-149">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="ac363-150">tooopen hello **konfigurace** kartě v hello Azure classic portálu, vyberte **služby Active Directory**a pak vyberte název hello hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="ac363-150">tooopen hello **Configure** tab in hello Azure classic portal, select **Active Directory**, and then select hello name of hello directory.</span></span>

![Karta Konfigurace ve službě Azure Active Directory][1]

<span data-ttu-id="ac363-152">Potom můžete upravit hello možnosti toocontrol přístupu uživatelů typu Host.</span><span class="sxs-lookup"><span data-stu-id="ac363-152">Then you can edit hello options toocontrol access for guest users.</span></span>

![možnosti řízení přístupu pro uživatele typu host][2]

## <a name="whats-next"></a><span data-ttu-id="ac363-154">Kam dál</span><span class="sxs-lookup"><span data-stu-id="ac363-154">What's next</span></span>
* [<span data-ttu-id="ac363-155">Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac363-155">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="ac363-156">Správa služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac363-156">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="ac363-157">Správa hesel ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac363-157">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="ac363-158">Správa skupin ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac363-158">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
