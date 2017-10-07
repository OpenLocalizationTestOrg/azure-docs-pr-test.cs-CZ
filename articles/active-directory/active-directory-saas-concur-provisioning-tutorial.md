---
title: 'Kurz: Azure Active Directory integrace s Concur | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Concur."
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
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="38457-103">Kurz: Konfigurace vyústit pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="38457-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="38457-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Concur a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooConcur Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38457-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Concur and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooConcur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38457-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="38457-105">Prerequisites</span></span>

<span data-ttu-id="38457-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="38457-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="38457-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="38457-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="38457-108">Concur jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="38457-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="38457-109">Uživatelský účet v Concur s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="38457-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-tooconcur"></a><span data-ttu-id="38457-110">Přiřazení uživatelů tooConcur</span><span class="sxs-lookup"><span data-stu-id="38457-110">Assigning users tooConcur</span></span>

<span data-ttu-id="38457-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="38457-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="38457-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38457-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="38457-113">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Concur tooyour.</span><span class="sxs-lookup"><span data-stu-id="38457-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Concur app.</span></span> <span data-ttu-id="38457-114">Jakmile se rozhodli, můžete přiřadit tyto aplikace Concur tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="38457-114">Once decided, you can assign these users tooyour Concur app by following hello instructions here:</span></span>

[<span data-ttu-id="38457-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="38457-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a><span data-ttu-id="38457-116">Důležité tipy pro přiřazení uživatelů tooConcur</span><span class="sxs-lookup"><span data-stu-id="38457-116">Important tips for assigning users tooConcur</span></span>

*   <span data-ttu-id="38457-117">Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooConcur zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="38457-117">It is recommended that a single Azure AD user be assigned tooConcur tootest hello provisioning configuration.</span></span> <span data-ttu-id="38457-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="38457-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="38457-119">Při přiřazování tooConcur uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="38457-119">When assigning a user tooConcur, you must select a valid user role.</span></span> <span data-ttu-id="38457-120">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="38457-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="38457-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="38457-121">Enable user provisioning</span></span>

<span data-ttu-id="38457-122">Tato část vás provede připojením vaší služby Azure AD tooConcur uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Concur podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38457-122">This section guides you through connecting your Azure AD tooConcur's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="38457-123">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Concur, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="38457-123">You may also choose tooenabled SAML-based Single Sign-On for Concur, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="38457-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="38457-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="38457-125">tooconfigure uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="38457-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="38457-126">Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooConcur.</span><span class="sxs-lookup"><span data-stu-id="38457-126">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooConcur.</span></span>

<span data-ttu-id="38457-127">tooenable aplikací v hello služby výdaje, že má toobe správné nastavení a použití profilu Správce webu služby.</span><span class="sxs-lookup"><span data-stu-id="38457-127">tooenable apps in hello Expense Service, there has toobe proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="38457-128">Nepřidáte hello WS správce role tooyour stávající profil správce, můžete použít pro funkce správy T & E.</span><span class="sxs-lookup"><span data-stu-id="38457-128">Don't add hello WS Admin role tooyour existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="38457-129">Vyústit konzultanty nebo správce klienta hello musíte vytvořit profil odlišné správce webové služby a správce klienta hello musí používat tento profil pro funkce služby správce webu hello (například povolení aplikace).</span><span class="sxs-lookup"><span data-stu-id="38457-129">Concur Consultants or hello client administrator must create a distinct Web Service Administrator profile and hello Client administrator must use this profile for hello Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="38457-130">Tyto profily musí být udržovány odděleně od správce klienta hello denní T & E profil správce (hello T & E správce profil by neměl mít přiřazenou roli WSAdmin hello).</span><span class="sxs-lookup"><span data-stu-id="38457-130">These profiles must be kept separate from hello client administrator's daily T&E admin profile (hello T&E admin profile should not have hello WSAdmin role assigned).</span></span>

<span data-ttu-id="38457-131">Když vytvoříte toobe profil hello používá k povolení aplikace hello, zadejte do pole profil uživatele hello jméno správce klienta hello.</span><span class="sxs-lookup"><span data-stu-id="38457-131">When you create hello profile toobe used for enabling hello app, enter hello client administrator's name into hello user profile fields.</span></span> <span data-ttu-id="38457-132">Tím se přiřadí vlastnictví toohello profilu.</span><span class="sxs-lookup"><span data-stu-id="38457-132">This assigns ownership toohello profile.</span></span> <span data-ttu-id="38457-133">Po vytvoření jednoho nebo více profilů hello klienta musí přihlásit se přes tento profil tooclick hello "*povolit*" tlačítko partnera aplikace v rámci nabídky hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="38457-133">Once one or more profiles is created, hello client must log in with this profile tooclick hello "*Enable*" button for a Partner App within hello Web Services menu.</span></span>

<span data-ttu-id="38457-134">Pro hello následující z důvodů by neměla provést tuto akci s hello profilu, které používají pro správu normální T & E.</span><span class="sxs-lookup"><span data-stu-id="38457-134">For hello following reasons, this action should not be done with hello profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="38457-135">Hello má klient toobe hello jednu, která klikne na možnost "*Ano*" v dialogu okně hello, které se zobrazí po povolení aplikace.</span><span class="sxs-lookup"><span data-stu-id="38457-135">hello client has toobe hello one that clicks "*Yes*" on hello dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="38457-136">Klikněte na toto tlačítko uznává, že hello klienta je ochotni pro hello partnera aplikace tooaccess svá data, takže jste nebo hello partnera nelze kliknout, tlačítko Ano.</span><span class="sxs-lookup"><span data-stu-id="38457-136">This click acknowledges hello client is willing for hello Partner application tooaccess their data, so you or hello Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="38457-137">Pokud správce klienta, která povolila aplikaci pomocí hello T & E správce profil odejde hello společnosti (výsledkem hello profil se deaktivovány), všechny aplikace povoleno pomocí tohoto profilu nefunguje, dokud aplikace hello je povolená s jinou aktivní správce WS profil.</span><span class="sxs-lookup"><span data-stu-id="38457-137">If a client administrator that has enabled an app using hello T&E admin profile leaves hello company (resulting in hello profile being inactivated), any apps enabled using that profile does not function until hello app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="38457-138">Z tohoto důvodu je mají odlišné profily WS správce toocreate.</span><span class="sxs-lookup"><span data-stu-id="38457-138">This is why you are supposed toocreate distinct WS Admin profiles.</span></span>

* <span data-ttu-id="38457-139">Pokud správce odejde hello společnosti, název hello související toohello profilu WS správce může být změněné toohello nahrazení správce, v případě potřeby bez dopadu na hello povoleno, že aplikace, protože není nutné tento profil deaktivovány.</span><span class="sxs-lookup"><span data-stu-id="38457-139">If an administrator leaves hello company, hello name associated toohello WS Admin profile can be changed toohello replacement administrator if desired without impacting hello enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="38457-140">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="38457-140">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="38457-141">Přihlaste se tooyour **Concur** klienta.</span><span class="sxs-lookup"><span data-stu-id="38457-141">Log on tooyour **Concur** tenant.</span></span>

2. <span data-ttu-id="38457-142">Z hello **správy** nabídce vyberte možnost **webové služby**.</span><span class="sxs-lookup"><span data-stu-id="38457-142">From hello **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="38457-143">![Klienta Concur](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klienta")</span><span class="sxs-lookup"><span data-stu-id="38457-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="38457-144">Na levé straně, od hello hello **webové služby** podokně, vyberte **povolit aplikaci partnera**.</span><span class="sxs-lookup"><span data-stu-id="38457-144">On hello left side, from hello **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="38457-145">![Povolit aplikaci partnera](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "povolit partnera aplikace")</span><span class="sxs-lookup"><span data-stu-id="38457-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="38457-146">Z hello **povolit aplikaci** seznamu, vyberte **Azure Active Directory**a potom klikněte na **povolit**.</span><span class="sxs-lookup"><span data-stu-id="38457-146">From hello **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="38457-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span><span class="sxs-lookup"><span data-stu-id="38457-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="38457-148">Klikněte na tlačítko **Ano** tooclose hello **potvrďte akci** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="38457-148">Click **Yes** tooclose hello **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="38457-149">![Akci potvrďte](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "potvrzení akce")</span><span class="sxs-lookup"><span data-stu-id="38457-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="38457-150">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="38457-150">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="38457-151">Pokud jste již nakonfigurovali Concur pro jednotné přihlašování, vyhledejte instanci Concur pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="38457-151">If you have already configured Concur for single sign-on, search for your instance of Concur using hello search field.</span></span> <span data-ttu-id="38457-152">Jinak vyberte možnost **přidat** a vyhledejte **Concur** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="38457-152">Otherwise, select **Add** and search for **Concur** in hello application gallery.</span></span> <span data-ttu-id="38457-153">Vyberte Concur z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="38457-153">Select Concur from hello search results, and add it tooyour list of applications.</span></span>

8. <span data-ttu-id="38457-154">Vyberte instanci Concur a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="38457-154">Select your instance of Concur, then select hello **Provisioning** tab.</span></span>

9. <span data-ttu-id="38457-155">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="38457-155">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
 
    ![Zřizování](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="38457-157">V části hello **přihlašovací údaje správce** zadejte hello **uživatelské jméno** a hello **heslo** vaše Concur správce.</span><span class="sxs-lookup"><span data-stu-id="38457-157">Under hello **Admin Credentials** section, enter hello **user name** and hello **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="38457-158">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Concur aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38457-158">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Concur app.</span></span> <span data-ttu-id="38457-159">Pokud hello připojení selže, zajistěte, aby byl váš účet Concur oprávnění správce týmu.</span><span class="sxs-lookup"><span data-stu-id="38457-159">If hello connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="38457-160">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="38457-160">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

13. <span data-ttu-id="38457-161">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="38457-161">Click **Save.**</span></span>

14. <span data-ttu-id="38457-162">V části hello části mapování, vyberte **tooConcur synchronizaci uživatelů Azure Active Directory.**</span><span class="sxs-lookup"><span data-stu-id="38457-162">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooConcur.**</span></span>

15. <span data-ttu-id="38457-163">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooConcur Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38457-163">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooConcur.</span></span> <span data-ttu-id="38457-164">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Concur pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="38457-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Concur for update operations.</span></span> <span data-ttu-id="38457-165">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="38457-165">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="38457-166">tooenable hello zřizování služby Azure AD pro Concur, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="38457-166">tooenable hello Azure AD provisioning service for Concur, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

17. <span data-ttu-id="38457-167">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="38457-167">Click **Save.**</span></span>

<span data-ttu-id="38457-168">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="38457-168">You can now create a test account.</span></span> <span data-ttu-id="38457-169">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooConcur.</span><span class="sxs-lookup"><span data-stu-id="38457-169">Wait for up too20 minutes tooverify that hello account has been synchronized tooConcur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38457-170">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="38457-170">Additional resources</span></span>

* [<span data-ttu-id="38457-171">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="38457-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="38457-172">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="38457-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="38457-173">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="38457-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

