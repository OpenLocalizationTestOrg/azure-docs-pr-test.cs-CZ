---
title: "Kurz: Konfigurace Samanage pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooSamanage."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 6cb36d2cc6ce33da4f8ebba65d138bfd4f2aca9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a><span data-ttu-id="fed11-103">Kurz: Konfigurace Samanage pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="fed11-103">Tutorial: Configuring Samanage for Automatic User Provisioning</span></span>


<span data-ttu-id="fed11-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Samanage a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooSamanage Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fed11-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Samanage and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSamanage.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fed11-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fed11-105">Prerequisites</span></span>

<span data-ttu-id="fed11-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="fed11-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="fed11-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="fed11-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="fed11-108">Samanage klienta s hello [Professional plán](https://www.samanage.com/pricing/) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="fed11-108">A Samanage tenant with hello [Professional plan](https://www.samanage.com/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="fed11-109">Uživatelský účet v Samanage s oprávněními správce</span><span class="sxs-lookup"><span data-stu-id="fed11-109">A user account in Samanage with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="fed11-110">Hello Azure AD zřizování integrace spoléhá na hello [Samanage REST API](https://www.samanage.com/api/), která je k dispozici tooSamanage týmy na hello Professional plánu nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="fed11-110">hello Azure AD provisioning integration relies on hello [Samanage REST API](https://www.samanage.com/api/), which is available tooSamanage teams on hello Professional plan or better.</span></span>

## <a name="assigning-users-toosamanage"></a><span data-ttu-id="fed11-111">Přiřazení uživatelů tooSamanage</span><span class="sxs-lookup"><span data-stu-id="fed11-111">Assigning users tooSamanage</span></span>

<span data-ttu-id="fed11-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="fed11-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="fed11-113">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fed11-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="fed11-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Samanage tooyour.</span><span class="sxs-lookup"><span data-stu-id="fed11-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Samanage app.</span></span> <span data-ttu-id="fed11-115">Jakmile se rozhodli, můžete přiřadit tyto aplikace Samanage tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="fed11-115">Once decided, you can assign these users tooyour Samanage app by following hello instructions here:</span></span>

[<span data-ttu-id="fed11-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="fed11-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toosamanage"></a><span data-ttu-id="fed11-117">Důležité tipy pro přiřazení uživatelů tooSamanage</span><span class="sxs-lookup"><span data-stu-id="fed11-117">Important tips for assigning users tooSamanage</span></span>

*   <span data-ttu-id="fed11-118">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooSamanage zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fed11-118">It is recommended that a single Azure AD user is assigned tooSamanage tootest hello provisioning configuration.</span></span> <span data-ttu-id="fed11-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="fed11-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="fed11-120">Při přiřazování tooSamanage uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="fed11-120">When assigning a user tooSamanage, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="fed11-121">Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="fed11-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="fed11-122">Jako přidané funkce hello zřizování služby načte všechny vlastní role, které jsou definované v Samanage a naimportuje je do Azure AD, kde mohou být vybrány v dialogovém okně vybrat roli hello.</span><span class="sxs-lookup"><span data-stu-id="fed11-122">As an added feature, hello provisioning service reads any custom roles defined in Samanage, and imports them into Azure AD where they can be selected in hello Select Role dialog.</span></span> <span data-ttu-id="fed11-123">Tyto role se nebude zobrazovat v hello portál Azure po povolení hello zřizování služby a jeden synchronizační cyklus byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="fed11-123">These roles will be visible in hello Azure portal after hello provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-toosamanage"></a><span data-ttu-id="fed11-124">Konfigurace tooSamanage zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="fed11-124">Configuring user provisioning tooSamanage</span></span> 

<span data-ttu-id="fed11-125">Tato část vás provede připojením vaší služby Azure AD tooSamanage uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Samanage podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fed11-125">This section guides you through connecting your Azure AD tooSamanage's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Samanage based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="fed11-126">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Samanage, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fed11-126">You may also choose tooenabled SAML-based Single Sign-On for Samanage, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="fed11-127">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="fed11-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toosamanage-in-azure-ad"></a><span data-ttu-id="fed11-128">Konfigurace automatického uživatelský účet zřizování tooSamanage ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="fed11-128">Configure automatic user account provisioning tooSamanage in Azure AD:</span></span>


1. <span data-ttu-id="fed11-129">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="fed11-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="fed11-130">Pokud jste již nakonfigurovali Samanage pro jednotné přihlašování, vyhledejte instanci Samanage pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="fed11-130">If you have already configured Samanage for single sign-on, search for your instance of Samanage using hello search field.</span></span> <span data-ttu-id="fed11-131">Jinak vyberte možnost **přidat** a vyhledejte **Samanage** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="fed11-131">Otherwise, select **Add** and search for **Samanage** in hello application gallery.</span></span> <span data-ttu-id="fed11-132">Vyberte Samanage z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="fed11-132">Select Samanage from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="fed11-133">Vyberte instanci Samanage a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="fed11-133">Select your instance of Samanage, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="fed11-134">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="fed11-134">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Zřizování Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. <span data-ttu-id="fed11-136">V části hello **přihlašovací údaje správce** část, vstupní hello **uživatelské jméno správce a heslo správce** vaše Samanage účtu.</span><span class="sxs-lookup"><span data-stu-id="fed11-136">Under hello **Admin Credentials** section, input hello **Admin Username&Admin Password** of your Samanage's account.</span></span> 

6. <span data-ttu-id="fed11-137">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Samanage aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fed11-137">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Samanage app.</span></span> <span data-ttu-id="fed11-138">Pokud hello připojení selže, zajistěte, aby byl váš účet Samanage oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="fed11-138">If hello connection fails, ensure your Samanage account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="fed11-139">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="fed11-139">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="fed11-140">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fed11-140">Click **Save**.</span></span> 

9. <span data-ttu-id="fed11-141">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooSamanage**.</span><span class="sxs-lookup"><span data-stu-id="fed11-141">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSamanage**.</span></span>

10. <span data-ttu-id="fed11-142">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooSamanage Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fed11-142">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSamanage.</span></span> <span data-ttu-id="fed11-143">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Samanage pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="fed11-143">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Samanage for update operations.</span></span> <span data-ttu-id="fed11-144">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="fed11-144">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="fed11-145">tooenable hello zřizování služby Azure AD pro Samanage, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="fed11-145">tooenable hello Azure AD provisioning service for Samanage, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="fed11-146">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fed11-146">Click **Save**.</span></span> 

<span data-ttu-id="fed11-147">Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooSamanage v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="fed11-147">This operation starts hello initial synchronization of any users and/or groups assigned tooSamanage in hello Users and Groups section.</span></span> <span data-ttu-id="fed11-148">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="fed11-148">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="fed11-149">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="fed11-149">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="fed11-150">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="fed11-150">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="fed11-151">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fed11-151">Additional resources</span></span>

* [<span data-ttu-id="fed11-152">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="fed11-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="fed11-153">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fed11-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="fed11-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fed11-154">Next steps</span></span>

* [<span data-ttu-id="fed11-155">Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="fed11-155">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
