---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="7d7a8-103">Kurz: Konfigurace Jive pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="7d7a8-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="7d7a8-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Jive a Azure AD do automaticky zřizovat a deaktivace zřízení uživatelských účtů ze služby Azure AD k Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-104">The objective of this tutorial is to show you the steps you need to perform in Jive and Azure AD to automatically provision and de-provision user accounts from Azure AD to Jive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d7a8-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7d7a8-105">Prerequisites</span></span>

<span data-ttu-id="7d7a8-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="7d7a8-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="7d7a8-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="7d7a8-108">Jive jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="7d7a8-109">Uživatelský účet v Jive s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-to-jive"></a><span data-ttu-id="7d7a8-110">Přiřazení uživatelů k Jive</span><span class="sxs-lookup"><span data-stu-id="7d7a8-110">Assigning users to Jive</span></span>

<span data-ttu-id="7d7a8-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="7d7a8-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="7d7a8-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Jive app.</span></span> <span data-ttu-id="7d7a8-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Jive podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="7d7a8-114">Once decided, you can assign these users to your Jive app by following the instructions here:</span></span>

[<span data-ttu-id="7d7a8-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="7d7a8-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a><span data-ttu-id="7d7a8-116">Důležité tipy pro přiřazování uživatelů do Jive</span><span class="sxs-lookup"><span data-stu-id="7d7a8-116">Important tips for assigning users to Jive</span></span>

*   <span data-ttu-id="7d7a8-117">Dále je doporučeno jednoho uživatele Azure AD pro Jive přidělí otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-117">It is recommended that a single Azure AD user be assigned to Jive to test the provisioning configuration.</span></span> <span data-ttu-id="7d7a8-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="7d7a8-119">Při přiřazování Jive uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-119">When assigning a user to Jive, you must select a valid user role.</span></span> <span data-ttu-id="7d7a8-120">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="7d7a8-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="7d7a8-121">Enable User Provisioning</span></span>

<span data-ttu-id="7d7a8-122">Tato část vás provede připojení k Jive na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Jive podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-122">This section guides you through connecting your Azure AD to Jive's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="7d7a8-123">Můžete také povolit na základě SAML jednotné přihlašování pro Jive, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7d7a8-123">You may also choose to enabled SAML-based Single Sign-On for Jive, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7d7a8-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="7d7a8-125">Ke konfiguraci zřizování účtu uživatele:</span><span class="sxs-lookup"><span data-stu-id="7d7a8-125">To configure user account provisioning:</span></span>

<span data-ttu-id="7d7a8-126">Cílem této části se popisují postup povolení zřizování uživatelů z uživatelských účtů služby Active Directory pro Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Jive.</span></span>
<span data-ttu-id="7d7a8-127">V rámci tohoto postupu jsou nezbytné, které budete muset požádat Jive.com token zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-127">As part of this procedure, you are required to provide a user security token you need to request from Jive.com.</span></span>

1. <span data-ttu-id="7d7a8-128">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-128">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="7d7a8-129">Pokud jste již nakonfigurovali Jive pro jednotné přihlašování, vyhledejte instanci Jive pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-129">If you have already configured Jive for single sign-on, search for your instance of Jive using the search field.</span></span> <span data-ttu-id="7d7a8-130">Jinak vyberte možnost **přidat** a vyhledejte **Jive** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-130">Otherwise, select **Add** and search for **Jive** in the application gallery.</span></span> <span data-ttu-id="7d7a8-131">Vyberte Jive ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-131">Select Jive from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="7d7a8-132">Vyberte instanci Jive a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-132">Select your instance of Jive, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="7d7a8-133">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-133">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="7d7a8-135">V části **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7d7a8-135">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="7d7a8-136">a.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-136">a.</span></span> <span data-ttu-id="7d7a8-137">V **uživatelské jméno správce Jive** textovému poli, zadejte název, který má účtu Jive **správce systému** profil v Jive.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-137">In the **Jive Admin User Name** textbox, type a Jive account name that has the **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="7d7a8-138">b.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-138">b.</span></span> <span data-ttu-id="7d7a8-139">V **heslo správce Jive** textovému poli, zadejte heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-139">In the **Jive Admin Password** textbox, type the password for this account.</span></span>
   
    <span data-ttu-id="7d7a8-140">c.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-140">c.</span></span> <span data-ttu-id="7d7a8-141">V **URL klienta Jive** textovému poli, zadejte adresu URL Jive klienta.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-141">In the **Jive Tenant URL** textbox, type the Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="7d7a8-142">Adresa URL Jive klienta je adresa URL, která je vaše organizace používá k přihlášení do Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-142">The Jive tenant URL is URL that is used by your organization to log in to Jive.</span></span>  
      > <span data-ttu-id="7d7a8-143">Obvykle se adresa URL má následující formát: **www.\< organizace\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-143">Typically, the URL has the following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="7d7a8-144">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-144">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Jive app.</span></span>

7. <span data-ttu-id="7d7a8-145">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtněte políčko níže.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-145">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

8. <span data-ttu-id="7d7a8-146">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="7d7a8-146">Click **Save.**</span></span>

9. <span data-ttu-id="7d7a8-147">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Jive.**</span><span class="sxs-lookup"><span data-stu-id="7d7a8-147">Under the Mappings section, select **Synchronize Azure Active Directory Users to Jive.**</span></span>

10. <span data-ttu-id="7d7a8-148">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-148">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Jive.</span></span> <span data-ttu-id="7d7a8-149">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Jive pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-149">The attributes selected as **Matching** properties are used to match the user accounts in Jive for update operations.</span></span> <span data-ttu-id="7d7a8-150">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-150">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="7d7a8-151">Povolit zřizování služby pro Jive Azure AD, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="7d7a8-151">To enable the Azure AD provisioning service for Jive, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="7d7a8-152">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="7d7a8-152">Click **Save.**</span></span>

<span data-ttu-id="7d7a8-153">Spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené k Jive v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-153">It starts the initial synchronization of any users and/or groups assigned to Jive in the Users and Groups section.</span></span> <span data-ttu-id="7d7a8-154">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-154">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="7d7a8-155">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce prováděné při zřizování služby ve vaší aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-155">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Jive app.</span></span>

<span data-ttu-id="7d7a8-156">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-156">You can now create a test account.</span></span> <span data-ttu-id="7d7a8-157">Chcete-li ověřit, že účet byly synchronizovány Jive Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="7d7a8-157">Wait for up to 20 minutes to verify that the account has been synchronized to Jive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d7a8-158">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7d7a8-158">Additional resources</span></span>

* [<span data-ttu-id="7d7a8-159">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="7d7a8-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d7a8-160">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d7a8-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7d7a8-161">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d7a8-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)