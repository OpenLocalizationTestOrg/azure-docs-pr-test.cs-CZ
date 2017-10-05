---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 3b509ffa934949200277ae431761d2accd4a02d6
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a><span data-ttu-id="1aea8-103">Kurz: Konfigurace DocuSign pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="1aea8-103">Tutorial: Configuring DocuSign for User Provisioning</span></span>

<span data-ttu-id="1aea8-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v DocuSign a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-104">The objective of this tutorial is to show you the steps you need to perform in DocuSign and Azure AD to automatically provision and de-provision user accounts from Azure AD to DocuSign.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aea8-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1aea8-105">Prerequisites</span></span>

<span data-ttu-id="1aea8-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="1aea8-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="1aea8-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="1aea8-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="1aea8-108">DocuSign jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="1aea8-108">A DocuSign single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="1aea8-109">Uživatelský účet v DocuSign s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="1aea8-109">A user account in DocuSign with Team Admin permissions.</span></span>

## <a name="assigning-users-to-docusign"></a><span data-ttu-id="1aea8-110">Přiřazení uživatelů k DocuSign</span><span class="sxs-lookup"><span data-stu-id="1aea8-110">Assigning users to DocuSign</span></span>

<span data-ttu-id="1aea8-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="1aea8-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="1aea8-112">V kontextu uživatele automatické zřizování účtu jsou synchronizovány pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aea8-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="1aea8-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your DocuSign app.</span></span> <span data-ttu-id="1aea8-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace DocuSign podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="1aea8-114">Once decided, you can assign these users to your DocuSign app by following the instructions here:</span></span>

[<span data-ttu-id="1aea8-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="1aea8-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a><span data-ttu-id="1aea8-116">Důležité tipy pro přiřazování uživatelů do DocuSign</span><span class="sxs-lookup"><span data-stu-id="1aea8-116">Important tips for assigning users to DocuSign</span></span>

*   <span data-ttu-id="1aea8-117">Dále je doporučeno jednoho uživatele Azure AD se přiřadí ke DocuSign a otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="1aea8-117">It is recommended that a single Azure AD user is assigned to DocuSign to test the provisioning configuration.</span></span> <span data-ttu-id="1aea8-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="1aea8-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="1aea8-119">Při přiřazování DocuSign uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="1aea8-119">When assigning a user to DocuSign, you must select a valid user role.</span></span> <span data-ttu-id="1aea8-120">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="1aea8-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="1aea8-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="1aea8-121">Enable User Provisioning</span></span>

<span data-ttu-id="1aea8-122">Tato část vás provede připojení k DocuSign na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v DocuSign podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aea8-122">This section guides you through connecting your Azure AD to DocuSign's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in DocuSign based on user and group assignment in Azure AD.</span></span>

> [!Tip]
> <span data-ttu-id="1aea8-123">Můžete také pro DocuSign povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1aea8-123">You may also choose to enabled SAML-based Single Sign-On for DocuSign, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1aea8-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="1aea8-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="1aea8-125">Ke konfiguraci zřizování účtu uživatele:</span><span class="sxs-lookup"><span data-stu-id="1aea8-125">To configure user account provisioning:</span></span>

<span data-ttu-id="1aea8-126">Cílem této části se popisují postup povolení zřizování uživatelů z uživatelských účtů služby Active Directory pro DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to DocuSign.</span></span>

1. <span data-ttu-id="1aea8-127">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="1aea8-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="1aea8-128">Pokud jste již nakonfigurovali DocuSign pro jednotné přihlašování, vyhledejte instanci DocuSign pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="1aea8-128">If you have already configured DocuSign for single sign-on, search for your instance of DocuSign using the search field.</span></span> <span data-ttu-id="1aea8-129">Jinak vyberte možnost **přidat** a vyhledejte **DocuSign** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="1aea8-129">Otherwise, select **Add** and search for **DocuSign** in the application gallery.</span></span> <span data-ttu-id="1aea8-130">Vyberte DocuSign ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="1aea8-130">Select DocuSign from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="1aea8-131">Vyberte instanci DocuSign a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="1aea8-131">Select your instance of DocuSign, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="1aea8-132">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="1aea8-132">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="1aea8-134">V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1aea8-134">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="1aea8-135">a.</span><span class="sxs-lookup"><span data-stu-id="1aea8-135">a.</span></span> <span data-ttu-id="1aea8-136">V **uživatelské jméno správce** textovému poli, zadejte název, který má účtu DocuSign **správce systému** profil v DocuSign.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="1aea8-136">In the **Admin User Name** textbox, type a DocuSign account name that has the **System Administrator** profile in DocuSign.com assigned.</span></span>
   
    <span data-ttu-id="1aea8-137">b.</span><span class="sxs-lookup"><span data-stu-id="1aea8-137">b.</span></span> <span data-ttu-id="1aea8-138">V **heslo správce** textovému poli, zadejte heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="1aea8-138">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="1aea8-139">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-139">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your DocuSign app.</span></span>

7. <span data-ttu-id="1aea8-140">V **e-mailové oznámení** pole, zadejte e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko.</span><span class="sxs-lookup"><span data-stu-id="1aea8-140">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

8. <span data-ttu-id="1aea8-141">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="1aea8-141">Click **Save.**</span></span>

9. <span data-ttu-id="1aea8-142">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům DocuSign.**</span><span class="sxs-lookup"><span data-stu-id="1aea8-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to DocuSign.**</span></span>

10. <span data-ttu-id="1aea8-143">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to DocuSign.</span></span> <span data-ttu-id="1aea8-144">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v DocuSign pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1aea8-144">The attributes selected as **Matching** properties are used to match the user accounts in DocuSign for update operations.</span></span> <span data-ttu-id="1aea8-145">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="1aea8-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="1aea8-146">Povolit zřizování služby pro DocuSign Azure AD, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="1aea8-146">To enable the Azure AD provisioning service for DocuSign, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="1aea8-147">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="1aea8-147">Click **Save.**</span></span>

<span data-ttu-id="1aea8-148">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k DocuSign v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="1aea8-148">It starts the initial synchronization of any users and/or groups assigned to DocuSign in the Users and Groups section.</span></span> <span data-ttu-id="1aea8-149">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="1aea8-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="1aea8-150">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="1aea8-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your DocuSign app.</span></span>

<span data-ttu-id="1aea8-151">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="1aea8-151">You can now create a test account.</span></span> <span data-ttu-id="1aea8-152">Chcete-li ověřit, že účet byly synchronizovány DocuSign Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="1aea8-152">Wait for up to 20 minutes to verify that the account has been synchronized to DocuSign.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1aea8-153">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1aea8-153">Additional resources</span></span>

* [<span data-ttu-id="1aea8-154">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="1aea8-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1aea8-155">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1aea8-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="1aea8-156">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1aea8-156">Configure Single Sign-on</span></span>](active-directory-saas-docusign-tutorial.md)