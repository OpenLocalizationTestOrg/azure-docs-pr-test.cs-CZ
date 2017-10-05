---
title: 'Kurz: Azure Active Directory integrace s Concur | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: cd35b6e2dc3171e9cffdb820bbc5b0d45ff58e07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="00cd8-103">Kurz: Konfigurace vyústit pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="00cd8-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="00cd8-104">Cílem tohoto kurzu je tak, aby zobrazovalo kroky, které je třeba provést v Concur a Azure AD a automaticky zřizovat a zrušte zřízení uživatelských účtů ze služby Azure AD do Concur.</span><span class="sxs-lookup"><span data-stu-id="00cd8-104">The objective of this tutorial is to show you the steps you need to perform in Concur and Azure AD to automatically provision and de-provision user accounts from Azure AD to Concur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00cd8-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00cd8-105">Prerequisites</span></span>

<span data-ttu-id="00cd8-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="00cd8-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="00cd8-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="00cd8-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="00cd8-108">Concur jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="00cd8-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="00cd8-109">Uživatelský účet v Concur s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="00cd8-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-to-concur"></a><span data-ttu-id="00cd8-110">Přiřazení uživatelů k Concur</span><span class="sxs-lookup"><span data-stu-id="00cd8-110">Assigning users to Concur</span></span>

<span data-ttu-id="00cd8-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="00cd8-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="00cd8-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00cd8-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="00cd8-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci Concur.</span><span class="sxs-lookup"><span data-stu-id="00cd8-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Concur app.</span></span> <span data-ttu-id="00cd8-114">Jakmile se rozhodli, můžete přiřadit těmto uživatelům aplikace Concur podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="00cd8-114">Once decided, you can assign these users to your Concur app by following the instructions here:</span></span>

[<span data-ttu-id="00cd8-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="00cd8-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a><span data-ttu-id="00cd8-116">Důležité tipy pro přiřazování uživatelů do Concur</span><span class="sxs-lookup"><span data-stu-id="00cd8-116">Important tips for assigning users to Concur</span></span>

*   <span data-ttu-id="00cd8-117">Dále je doporučeno jednoho uživatele Azure AD pro Concur přidělí otestovat konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="00cd8-117">It is recommended that a single Azure AD user be assigned to Concur to test the provisioning configuration.</span></span> <span data-ttu-id="00cd8-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="00cd8-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="00cd8-119">Při přiřazování Concur uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="00cd8-119">When assigning a user to Concur, you must select a valid user role.</span></span> <span data-ttu-id="00cd8-120">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="00cd8-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="00cd8-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="00cd8-121">Enable user provisioning</span></span>

<span data-ttu-id="00cd8-122">Tato část vás provede připojení k Concur na uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v Concur podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00cd8-122">This section guides you through connecting your Azure AD to Concur's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="00cd8-123">Můžete také pro Concur povoleno na základě SAML jednotné přihlašování, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="00cd8-123">You may also choose to enabled SAML-based Single Sign-On for Concur, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="00cd8-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="00cd8-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="00cd8-125">Ke konfiguraci zřizování účtu uživatele:</span><span class="sxs-lookup"><span data-stu-id="00cd8-125">To configure user account provisioning:</span></span>

<span data-ttu-id="00cd8-126">Cílem této části se popisují postup povolení zřizování uživatelských účtů služby Active Directory do Concur.</span><span class="sxs-lookup"><span data-stu-id="00cd8-126">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Concur.</span></span>

<span data-ttu-id="00cd8-127">K povolení služby náklady, že musí být správné nastavení a použití profilu Správce webu služby.</span><span class="sxs-lookup"><span data-stu-id="00cd8-127">To enable apps in the Expense Service, there has to be proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="00cd8-128">Nepřidáte roli správce WS stávající profil správce, který používáte pro T & E funkce správy.</span><span class="sxs-lookup"><span data-stu-id="00cd8-128">Don't add the WS Admin role to your existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="00cd8-129">Vyústit konzultanty nebo správce klienta, musíte vytvořit profil odlišné správce webové služby a musí správce klienta použít tento profil pro funkce správce webu služby (například povolení aplikace).</span><span class="sxs-lookup"><span data-stu-id="00cd8-129">Concur Consultants or the client administrator must create a distinct Web Service Administrator profile and the Client administrator must use this profile for the Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="00cd8-130">Tyto profily musí být udržovány odděleně od klienta denní T & E správce profil správce (Správce profil T & E by neměl mít přiřazenou roli WSAdmin).</span><span class="sxs-lookup"><span data-stu-id="00cd8-130">These profiles must be kept separate from the client administrator's daily T&E admin profile (the T&E admin profile should not have the WSAdmin role assigned).</span></span>

<span data-ttu-id="00cd8-131">Když vytvoříte profil, který se použije pro povolení aplikace, zadejte do pole profil uživatele jméno správce klienta.</span><span class="sxs-lookup"><span data-stu-id="00cd8-131">When you create the profile to be used for enabling the app, enter the client administrator's name into the user profile fields.</span></span> <span data-ttu-id="00cd8-132">Tím se přiřadí vlastnictví k profilu.</span><span class="sxs-lookup"><span data-stu-id="00cd8-132">This assigns ownership to the profile.</span></span> <span data-ttu-id="00cd8-133">Po vytvoření jednoho nebo více profilů klient musí přihlásit pomocí tohoto profilu můžete kliknutím na "*povolit*" tlačítko pro partnera aplikaci v nabídce webové služby.</span><span class="sxs-lookup"><span data-stu-id="00cd8-133">Once one or more profiles is created, the client must log in with this profile to click the "*Enable*" button for a Partner App within the Web Services menu.</span></span>

<span data-ttu-id="00cd8-134">Z následujících důvodů by neměla provést tuto akci s profilem, které používají pro správu normální T & E.</span><span class="sxs-lookup"><span data-stu-id="00cd8-134">For the following reasons, this action should not be done with the profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="00cd8-135">Klient musí být ten, který klikne na možnost "*Ano*" v dialogu okně, které se zobrazí po povolení aplikace.</span><span class="sxs-lookup"><span data-stu-id="00cd8-135">The client has to be the one that clicks "*Yes*" on the dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="00cd8-136">Klikněte na toto tlačítko uznává, že klient je ochotná partnera aplikace přistupovat ke svým datům, takže můžete nebo partnerovi nelze na toto tlačítko Ano.</span><span class="sxs-lookup"><span data-stu-id="00cd8-136">This click acknowledges the client is willing for the Partner application to access their data, so you or the Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="00cd8-137">Pokud správce klienta, která povolila aplikaci pomocí Správce T & E profil společnost opustí (což je v profilu se deaktivovány), musí všechny aplikace povolit pomocí profilu dokud aplikace je povolená s jinou active profilu WS správce nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="00cd8-137">If a client administrator that has enabled an app using the T&E admin profile leaves the company (resulting in the profile being inactivated), any apps enabled using that profile does not function until the app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="00cd8-138">Z tohoto důvodu je mají odlišné správce WS vytváření profilů.</span><span class="sxs-lookup"><span data-stu-id="00cd8-138">This is why you are supposed to create distinct WS Admin profiles.</span></span>

* <span data-ttu-id="00cd8-139">Pokud správce ze společnosti odejde, název přidružené k profilu WS správce lze změnit správci nahrazení v případě potřeby bez dopadu, že aplikaci povoleno, protože není nutné tento profil deaktivovány.</span><span class="sxs-lookup"><span data-stu-id="00cd8-139">If an administrator leaves the company, the name associated to the WS Admin profile can be changed to the replacement administrator if desired without impacting the enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="00cd8-140">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="00cd8-140">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="00cd8-141">Přihlaste se k vaší **Concur** klienta.</span><span class="sxs-lookup"><span data-stu-id="00cd8-141">Log on to your **Concur** tenant.</span></span>

2. <span data-ttu-id="00cd8-142">Z **správy** nabídce vyberte možnost **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="00cd8-142">From the **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="00cd8-143">![Klienta Concur](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klienta")</span><span class="sxs-lookup"><span data-stu-id="00cd8-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="00cd8-144">Na levé straně od **webové služby** podokně, vyberte **povolit aplikaci partnera**.</span><span class="sxs-lookup"><span data-stu-id="00cd8-144">On the left side, from the **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="00cd8-145">![Povolit aplikaci partnera](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "povolit partnera aplikace")</span><span class="sxs-lookup"><span data-stu-id="00cd8-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="00cd8-146">Z **povolit aplikaci** seznamu, vyberte **Azure Active Directory**a potom klikněte na **povolit**.</span><span class="sxs-lookup"><span data-stu-id="00cd8-146">From the **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="00cd8-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span><span class="sxs-lookup"><span data-stu-id="00cd8-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="00cd8-148">Klikněte na tlačítko **Ano** zavřete **potvrďte akci** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="00cd8-148">Click **Yes** to close the **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="00cd8-149">![Akci potvrďte](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "potvrzení akce")</span><span class="sxs-lookup"><span data-stu-id="00cd8-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="00cd8-150">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="00cd8-150">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="00cd8-151">Pokud jste již nakonfigurovali Concur pro jednotné přihlašování, vyhledejte instanci Concur pomocí pole hledání.</span><span class="sxs-lookup"><span data-stu-id="00cd8-151">If you have already configured Concur for single sign-on, search for your instance of Concur using the search field.</span></span> <span data-ttu-id="00cd8-152">Jinak vyberte možnost **přidat** a vyhledejte **Concur** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="00cd8-152">Otherwise, select **Add** and search for **Concur** in the application gallery.</span></span> <span data-ttu-id="00cd8-153">Vyberte Concur ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="00cd8-153">Select Concur from the search results, and add it to your list of applications.</span></span>

8. <span data-ttu-id="00cd8-154">Vyberte instanci Concur a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="00cd8-154">Select your instance of Concur, then select the **Provisioning** tab.</span></span>

9. <span data-ttu-id="00cd8-155">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="00cd8-155">Set the **Provisioning Mode** to **Automatic**.</span></span> 
 
    ![Zřizování](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="00cd8-157">V části **přihlašovací údaje správce** zadejte **uživatelské jméno** a **heslo** vaše Concur správce.</span><span class="sxs-lookup"><span data-stu-id="00cd8-157">Under the **Admin Credentials** section, enter the **user name** and the **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="00cd8-158">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k aplikaci Concur.</span><span class="sxs-lookup"><span data-stu-id="00cd8-158">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Concur app.</span></span> <span data-ttu-id="00cd8-159">Pokud se nepovede připojit, zajistěte, aby byl váš účet Concur oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="00cd8-159">If the connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="00cd8-160">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="00cd8-160">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

13. <span data-ttu-id="00cd8-161">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="00cd8-161">Click **Save.**</span></span>

14. <span data-ttu-id="00cd8-162">V části mapování vyberte **synchronizaci Azure Active Directory uživatelům Concur.**</span><span class="sxs-lookup"><span data-stu-id="00cd8-162">Under the Mappings section, select **Synchronize Azure Active Directory Users to Concur.**</span></span>

15. <span data-ttu-id="00cd8-163">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD Concur.</span><span class="sxs-lookup"><span data-stu-id="00cd8-163">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Concur.</span></span> <span data-ttu-id="00cd8-164">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v Concur pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="00cd8-164">The attributes selected as **Matching** properties are used to match the user accounts in Concur for update operations.</span></span> <span data-ttu-id="00cd8-165">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="00cd8-165">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="00cd8-166">Povolit zřizování služby pro Concur Azure AD, změňte **Stav zřizování** k **na** v **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="00cd8-166">To enable the Azure AD provisioning service for Concur, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

17. <span data-ttu-id="00cd8-167">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="00cd8-167">Click **Save.**</span></span>

<span data-ttu-id="00cd8-168">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="00cd8-168">You can now create a test account.</span></span> <span data-ttu-id="00cd8-169">Chcete-li ověřit, že účet byly synchronizovány Concur Počkejte až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="00cd8-169">Wait for up to 20 minutes to verify that the account has been synchronized to Concur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00cd8-170">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="00cd8-170">Additional resources</span></span>

* [<span data-ttu-id="00cd8-171">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="00cd8-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00cd8-172">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="00cd8-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="00cd8-173">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00cd8-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

