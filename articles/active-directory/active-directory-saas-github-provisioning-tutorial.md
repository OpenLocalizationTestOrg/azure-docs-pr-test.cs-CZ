---
title: "Kurz: Konfigurace GitHub pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooGitHub."
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
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a><span data-ttu-id="86cfc-103">Kurz: Konfigurace GitHub pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="86cfc-103">Tutorial: Configuring GitHub for Automatic User Provisioning</span></span>


<span data-ttu-id="86cfc-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Githubu a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooGitHub Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86cfc-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in GitHub and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGitHub.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="86cfc-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="86cfc-105">Prerequisites</span></span>

<span data-ttu-id="86cfc-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="86cfc-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="86cfc-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="86cfc-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="86cfc-108">Github klienta s hello [obchodní plán](https://help.github.com/articles/organization-billing-plans/#business-plan) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="86cfc-108">A Github tenant with hello [Business plan](https://help.github.com/articles/organization-billing-plans/#business-plan) or better enabled</span></span> 
*   <span data-ttu-id="86cfc-109">Uživatelský účet na webu GitHub s oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="86cfc-109">A user account in GitHub with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="86cfc-110">Hello Azure AD zřizování integrace spoléhá na hello [Githubu SCIM API](https://developer.github.com/v3/scim/), která je k dispozici tooGithub týmy hello firmy plán nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="86cfc-110">hello Azure AD provisioning integration relies on hello [GitHub SCIM API](https://developer.github.com/v3/scim/), which is available tooGithub teams on hello Business plan or better.</span></span>

## <a name="assigning-users-toogithub"></a><span data-ttu-id="86cfc-111">Přiřazení uživatelů tooGitHub</span><span class="sxs-lookup"><span data-stu-id="86cfc-111">Assigning users tooGitHub</span></span>

<span data-ttu-id="86cfc-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="86cfc-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="86cfc-113">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86cfc-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="86cfc-114">Než nakonfigurujete a povolíte hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci tooyour Githubu.</span><span class="sxs-lookup"><span data-stu-id="86cfc-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour GitHub app.</span></span> <span data-ttu-id="86cfc-115">Jakmile se rozhodli, můžete přiřadit tyto aplikace Githubu tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="86cfc-115">Once decided, you can assign these users tooyour GitHub app by following hello instructions here:</span></span>

[<span data-ttu-id="86cfc-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="86cfc-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a><span data-ttu-id="86cfc-117">Důležité tipy pro přiřazení uživatelů tooGitHub</span><span class="sxs-lookup"><span data-stu-id="86cfc-117">Important tips for assigning users tooGitHub</span></span>

*   <span data-ttu-id="86cfc-118">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooGitHub zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="86cfc-118">It is recommended that a single Azure AD user is assigned tooGitHub tootest hello provisioning configuration.</span></span> <span data-ttu-id="86cfc-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="86cfc-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="86cfc-120">Při přiřazování tooGitHub uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="86cfc-120">When assigning a user tooGitHub, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="86cfc-121">Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="86cfc-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toogithub"></a><span data-ttu-id="86cfc-122">Konfigurace tooGitHub zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="86cfc-122">Configuring user provisioning tooGitHub</span></span> 

<span data-ttu-id="86cfc-123">Tato část vás provede připojením vaší služby Azure AD tooGitHub uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Githubu podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86cfc-123">This section guides you through connecting your Azure AD tooGitHub's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in GitHub based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="86cfc-124">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro GitHub, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="86cfc-124">You may also choose tooenabled SAML-based Single Sign-On for GitHub, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="86cfc-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="86cfc-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a><span data-ttu-id="86cfc-126">Konfigurace automatického uživatelského účtu zřizování tooGitHub ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="86cfc-126">Configure automatic user account provisioning tooGitHub in Azure AD</span></span>


1. <span data-ttu-id="86cfc-127">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="86cfc-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="86cfc-128">Pokud jste již nakonfigurovali GitHub pro jednotné přihlašování, vyhledejte instanci Githubu pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="86cfc-128">If you have already configured GitHub for single sign-on, search for your instance of GitHub using hello search field.</span></span> <span data-ttu-id="86cfc-129">Jinak vyberte možnost **přidat** a vyhledejte **Githubu** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="86cfc-129">Otherwise, select **Add** and search for **GitHub** in hello application gallery.</span></span> <span data-ttu-id="86cfc-130">Vyberte Githubu z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="86cfc-130">Select GitHub from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="86cfc-131">Vyberte instanci Githubu a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="86cfc-131">Select your instance of GitHub, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="86cfc-132">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Zřizování Githubu](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. <span data-ttu-id="86cfc-134">V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="86cfc-135">Tato operace otevře dialogové okno Githubu autorizace v nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="86cfc-135">This operation opens a GitHub authorization dialog in a new browser window.</span></span> 

6. <span data-ttu-id="86cfc-136">V novém okně hello se přihlaste pomocí účtu správce Githubu.</span><span class="sxs-lookup"><span data-stu-id="86cfc-136">In hello new window, sign into GitHub using your Admin account.</span></span> <span data-ttu-id="86cfc-137">V dialogu autorizace výsledné hello, vyberte hello Githubu týmu, který má být tooenable zřizování pro a potom vyberte **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-137">In hello resulting authorization dialog, select hello GitHub team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="86cfc-138">Po dokončení, vrátí toohello Azure portálu toocomplete hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="86cfc-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. <span data-ttu-id="86cfc-140">V hello portálu Azure, zadejte **URL klienta** a klikněte na tlačítko **Test připojení** tooensure Azure AD můžete připojit tooyour Githubu aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86cfc-140">In hello Azure portal, input **Tenant URL** and click **Test Connection** tooensure Azure AD can connect tooyour GitHub app.</span></span> <span data-ttu-id="86cfc-141">Pokud hello připojení nezdaří, ujistěte se, váš účet GitHub má oprávnění správce a **URl klienta** je správně zadané hodnoty a zkuste to znovu "Autorizace" krok text hello (mohou představovat **URL klienta** pravidlem: "https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ", vaší organizace můžete najít v rámci účtu GitHub: **nastavení** > **organizace**).</span><span class="sxs-lookup"><span data-stu-id="86cfc-141">If hello connection fails, ensure your GitHub account has Admin permissions and **Tenant URl** is inputted correctly, then try hello "Authorize" step again (you can constitute **Tenant URL** by rule: "https://api.github.com/scim/v2/organizations/ + <Organizations_name>", you can find your organizations under your GitHub account: **Settings** > **Organizations**).</span></span>

    ![Dialogové okno autorizace](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. <span data-ttu-id="86cfc-143">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="86cfc-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

9. <span data-ttu-id="86cfc-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-144">Click **Save**.</span></span> 

10. <span data-ttu-id="86cfc-145">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooGitHub**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGitHub**.</span></span>

11. <span data-ttu-id="86cfc-146">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooGitHub Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86cfc-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGitHub.</span></span> <span data-ttu-id="86cfc-147">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Githubu pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="86cfc-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in GitHub for update operations.</span></span> <span data-ttu-id="86cfc-148">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="86cfc-148">Select hello Save button toocommit any changes.</span></span>

12. <span data-ttu-id="86cfc-149">tooenable hello zřizování služby Azure AD pro GitHub, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="86cfc-149">tooenable hello Azure AD provisioning service for GitHub, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13. <span data-ttu-id="86cfc-150">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86cfc-150">Click **Save**.</span></span> 

<span data-ttu-id="86cfc-151">Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooGitHub v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="86cfc-151">This operation starts hello initial synchronization of any users and/or groups assigned tooGitHub in hello Users and Groups section.</span></span> <span data-ttu-id="86cfc-152">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="86cfc-152">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="86cfc-153">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="86cfc-153">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="86cfc-154">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="86cfc-154">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="86cfc-155">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="86cfc-155">Additional resources</span></span>

* [<span data-ttu-id="86cfc-156">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="86cfc-156">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="86cfc-157">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="86cfc-157">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="86cfc-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86cfc-158">Next steps</span></span>

* [<span data-ttu-id="86cfc-159">Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="86cfc-159">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
