---
title: "Přidání nových uživatelů do služby Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak ve službě Azure Active Directory přidat nové uživatele nebo změnit informace o uživatelích."
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
ms.openlocfilehash: ff4b742e772a6062885313e9bb49e55907fe125a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-to-azure-active-directory"></a><span data-ttu-id="01962-103">Přidání nových uživatelů nebo uživatelů s účty Microsoft do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01962-103">Add new users or users with Microsoft accounts to Azure Active Directory</span></span>
<span data-ttu-id="01962-104">Vyplňte svůj adresář uživateli, které nově přidáte.</span><span class="sxs-lookup"><span data-stu-id="01962-104">Add users to populate your directory.</span></span> <span data-ttu-id="01962-105">Tento článek vysvětluje, jak pro vaši organizaci přidat nové uživatele a jak přidat uživatele, kteří mají účty Microsoft.</span><span class="sxs-lookup"><span data-stu-id="01962-105">This article explains how to add new users in your organization, and how to add users who have Microsoft accounts.</span></span> <span data-ttu-id="01962-106">Další informace o přidávání uživatelů z dalších adresářů ve službě Azure Active Directory nebo přidávání uživatelů z partnerských společností najdete v článku o [přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="01962-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="01962-107">Přidaní uživatelé nemají ve výchozím nastavení oprávnění správce, ale příslušné role jim můžete kdykoli přiřadit.</span><span class="sxs-lookup"><span data-stu-id="01962-107">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="01962-108">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="01962-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="01962-109">Jak přidat uživatele v Centru správy služby Azure AD, najdete v článku [přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="01962-109">For how to add a user in the Azure AD admin center, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="01962-110">Přidání uživatele</span><span class="sxs-lookup"><span data-stu-id="01962-110">Add a user</span></span>
1. <span data-ttu-id="01962-111">Přihlaste se k portálu [Azure Classic](https://manage.windowsazure.com) prostřednictvím účtu, který má k adresáři oprávnění globálního správce.</span><span class="sxs-lookup"><span data-stu-id="01962-111">Sign in to the [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="01962-112">Vyberte **Active Directory** a potom vyberte název adresáře své organizace.</span><span class="sxs-lookup"><span data-stu-id="01962-112">Select **Active Directory**, and then select the name of your organization directory.</span></span>
3. <span data-ttu-id="01962-113">Vyberte kartu **Uživatelé** a na panelu příkazů vyberte **Přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="01962-113">Select the **Users** tab, and then, in the command bar, select **Add User**.</span></span>
4. <span data-ttu-id="01962-114">Na stránce **Informace o uživateli** vyberte v části **Typ uživatele** jedno z následujících:</span><span class="sxs-lookup"><span data-stu-id="01962-114">On the **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="01962-115">**Nový uživatel v organizaci** – přidá do vašeho adresáře nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="01962-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="01962-116">**Uživatel s existujícím účtem Microsoft** – přidá do vašeho adresáře existující účet Microsoft uživatele (například účet Outlook).</span><span class="sxs-lookup"><span data-stu-id="01962-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account to your directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="01962-117">V závislosti na tom, jaký **Typ uživatele** jste vybrali, zadejte buď uživatelské jméno (pro nového uživatele), nebo e-mailovou adresu (pro uživatele s účtem Microsoft).</span><span class="sxs-lookup"><span data-stu-id="01962-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="01962-118">Na stránce **Profil** uživatele zadejte jeho jméno, příjmení a uživatelské jméno a ze seznamu **Role** vyberte roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="01962-118">On the user **Profile** page, provide a first and last name, a user-friendly name, and a user role from the **Roles** list.</span></span> <span data-ttu-id="01962-119">Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="01962-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="01962-120">Určete, jestli u uživatele chcete **Povolit službu Multi-Factor Authentication**.</span><span class="sxs-lookup"><span data-stu-id="01962-120">Specify whether to **Enable Multi-Factor Authentication** for the user.</span></span>
7. <span data-ttu-id="01962-121">Na stránce **Získat dočasné heslo** vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01962-121">On the **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="01962-122">Pokud vaše organizace používá více než jednu doménu, měli byste vědět o následujících problémech týkajících se přidávání uživatelských účtů:</span><span class="sxs-lookup"><span data-stu-id="01962-122">If your organization uses more than one domain, you should know about the following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="01962-123">Pokud chcete přidat uživatelské účty s totožným hlavním názvem uživatele (UPN) pro všechny domény, přidejte například geoffgrisso@contoso.onmicrosoft.com **jako první** a **až potom** geoffgrisso@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="01962-123">TO add user accounts with the same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="01962-124">**Nepřidávejte** geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="01962-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com.</span></span> <span data-ttu-id="01962-125">Správné pořadí je důležité a náprava chyby může být náročná.</span><span class="sxs-lookup"><span data-stu-id="01962-125">This order is important, and can be cumbersome to undo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="01962-126">Změna informací o uživateli</span><span class="sxs-lookup"><span data-stu-id="01962-126">Change user information</span></span>
<span data-ttu-id="01962-127">S výjimkou ID objektu můžete změnit jakýkoli atribut uživatele.</span><span class="sxs-lookup"><span data-stu-id="01962-127">You can change any user attribute except for the object ID.</span></span>

1. <span data-ttu-id="01962-128">Otevřete svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="01962-128">Open your directory.</span></span>
2. <span data-ttu-id="01962-129">Vyberte kartu **Uživatelé** a pak vyberte zobrazované jméno uživatele, jehož informace chcete změnit.</span><span class="sxs-lookup"><span data-stu-id="01962-129">Select the **Users** tab, and then select the display name of the user you want to change.</span></span>
3. <span data-ttu-id="01962-130">Proveďte změny a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="01962-130">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="01962-131">Pokud je uživatel, jehož informace chcete změnit, synchronizovaný s místní službou Active Directory, nemůžete tento postup použít.</span><span class="sxs-lookup"><span data-stu-id="01962-131">If the user that you're changing is synchronized with your on-premises Active Directory service, you can't change the user information using this procedure.</span></span> <span data-ttu-id="01962-132">Ke změně informací o uživateli použijte nástroje pro správu vaší místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="01962-132">To change the user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="01962-133">Správa a omezení uživatele typu host</span><span class="sxs-lookup"><span data-stu-id="01962-133">Guest user management and limitations</span></span>
<span data-ttu-id="01962-134">Účty hostů jsou uživatelé z jiných adresářů, kteří byli do vašeho adresáře pozváni a mají přístup k dokumentům SharePointu, aplikacím a dalším prostředkům služby Azure.</span><span class="sxs-lookup"><span data-stu-id="01962-134">Guest accounts are users from other directories who were invited to your directory to access SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="01962-135">Účet hosta má ve vašem adresáři základní atribut UserType (Typ uživatele) nastavený na hodnotu Guest (Host).</span><span class="sxs-lookup"><span data-stu-id="01962-135">A guest account in your directory has its underlying UserType attribute set to "Guest."</span></span> <span data-ttu-id="01962-136">Běžní uživatelé (konkrétně členové vašeho adresáře) mají atribut UserType nastavený na hodnotu Member (Člen).</span><span class="sxs-lookup"><span data-stu-id="01962-136">Regular users (specifically, members of your directory) have the UserType attribute "Member."</span></span>

<span data-ttu-id="01962-137">Hosté mají ve vašem adresáři omezená oprávnění.</span><span class="sxs-lookup"><span data-stu-id="01962-137">Guests have a limited set of rights in the directory.</span></span> <span data-ttu-id="01962-138">Hostům jejich oprávnění neumožňují získávat podrobnější informace o ostatních uživatelích v adresáři.</span><span class="sxs-lookup"><span data-stu-id="01962-138">These rights limit the ability for Guests to discover information about other users in the directory.</span></span> <span data-ttu-id="01962-139">Uživatelé typu host však mohou interagovat s uživateli a skupinami přiřazenými k prostředkům, na kterých pracují.</span><span class="sxs-lookup"><span data-stu-id="01962-139">However, guest users can still interact with the users and groups associated with the resources they're working on.</span></span> <span data-ttu-id="01962-140">Uživatel typu host může:</span><span class="sxs-lookup"><span data-stu-id="01962-140">Guest users can:</span></span>

* <span data-ttu-id="01962-141">Vidět ostatní uživatele a skupiny přidružené k předplatnému Azure, ke kterému je přiřazený</span><span class="sxs-lookup"><span data-stu-id="01962-141">See other users and groups associated with an Azure subscription to which they're assigned</span></span>
* <span data-ttu-id="01962-142">Vidět členy skupin, do kterých patří</span><span class="sxs-lookup"><span data-stu-id="01962-142">See the members of groups to which they belong</span></span>
* <span data-ttu-id="01962-143">Vyhledávat ostatní uživatele v adresáři v případě, že zná jejich úplné e-mailové adresy</span><span class="sxs-lookup"><span data-stu-id="01962-143">Look up other users in the directory, if they know the full email address of the user</span></span>
* <span data-ttu-id="01962-144">Vidět omezenou sadu atributů uživatelů, které vyhledává – omezeno na zobrazované jméno, e-mailovou adresu, hlavní název uživatele (UPN) a miniaturu fotografie</span><span class="sxs-lookup"><span data-stu-id="01962-144">See only a limited set of attributes of the users they look up--limited to display name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="01962-145">Získat seznam ověřených domén v adresáři</span><span class="sxs-lookup"><span data-stu-id="01962-145">Get a list of verified domains in the directory</span></span>
* <span data-ttu-id="01962-146">Dávat souhlas aplikacím, udělovat jim stejným přístup, jako mají členové vašeho adresáře</span><span class="sxs-lookup"><span data-stu-id="01962-146">Consent to applications, granting them the same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="01962-147">Nastavení zásad přístupu uživatele typu host</span><span class="sxs-lookup"><span data-stu-id="01962-147">Set guest user access policies</span></span>
<span data-ttu-id="01962-148">Karta **Konfigurace** v adresáři zahrnuje i možnosti řízení přístupu uživatelů typu host.</span><span class="sxs-lookup"><span data-stu-id="01962-148">The **Configure** tab of a directory includes options to control access for guest users.</span></span> <span data-ttu-id="01962-149">Tyto možnosti může změnit pouze globální správce adresáře prostřednictvím portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="01962-149">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="01962-150">V současné době není k dispozici žádná metoda změny pomocí PowerShellu ani rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="01962-150">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="01962-151">Pokud chcete na portálu Azure Classic otevřít kartu **Konfigurace**, vyberte **Active Directory** a pak vyberte název adresáře.</span><span class="sxs-lookup"><span data-stu-id="01962-151">To open the **Configure** tab in the Azure classic portal, select **Active Directory**, and then select the name of the directory.</span></span>

![Karta Konfigurace ve službě Azure Active Directory][1]

<span data-ttu-id="01962-153">Potom můžete upravit možnosti řízení přístupu pro uživatele typu host.</span><span class="sxs-lookup"><span data-stu-id="01962-153">Then you can edit the options to control access for guest users.</span></span>

![možnosti řízení přístupu pro uživatele typu host][2]

## <a name="whats-next"></a><span data-ttu-id="01962-155">Kam dál</span><span class="sxs-lookup"><span data-stu-id="01962-155">What's next</span></span>
* [<span data-ttu-id="01962-156">Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01962-156">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="01962-157">Správa služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01962-157">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="01962-158">Správa hesel ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="01962-158">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="01962-159">Správa skupin ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="01962-159">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
