---
title: "Kurz: Konfigurace LucidChart pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooLucidChart."
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
ms.openlocfilehash: d3af45141731215f2edc8942ad21b016468c1e38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="97da3-103">Kurz: Konfigurace LucidChart pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="97da3-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="97da3-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v LucidChart a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooLucidChart Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97da3-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LucidChart and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="97da3-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97da3-105">Prerequisites</span></span>

<span data-ttu-id="97da3-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="97da3-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="97da3-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="97da3-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="97da3-108">LucidChart klienta s hello [plánu podnikového](https://www.lucidchart.com/user/117598685#/subscriptionLevel) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="97da3-108">A LucidChart tenant with hello [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="97da3-109">Uživatelský účet v LucidChart s oprávněními správce</span><span class="sxs-lookup"><span data-stu-id="97da3-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-toolucidchart"></a><span data-ttu-id="97da3-110">Přiřazení uživatelů tooLucidChart</span><span class="sxs-lookup"><span data-stu-id="97da3-110">Assigning users tooLucidChart</span></span>

<span data-ttu-id="97da3-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="97da3-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="97da3-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97da3-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="97da3-113">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci LucidChart tooyour.</span><span class="sxs-lookup"><span data-stu-id="97da3-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour LucidChart app.</span></span> <span data-ttu-id="97da3-114">Jakmile se rozhodli, můžete přiřadit tyto aplikace LucidChart tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="97da3-114">Once decided, you can assign these users tooyour LucidChart app by following hello instructions here:</span></span>

[<span data-ttu-id="97da3-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="97da3-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolucidchart"></a><span data-ttu-id="97da3-116">Důležité tipy pro přiřazení uživatelů tooLucidChart</span><span class="sxs-lookup"><span data-stu-id="97da3-116">Important tips for assigning users tooLucidChart</span></span>

*   <span data-ttu-id="97da3-117">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooLucidChart zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="97da3-117">It is recommended that a single Azure AD user is assigned tooLucidChart tootest hello provisioning configuration.</span></span> <span data-ttu-id="97da3-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="97da3-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="97da3-119">Při přiřazování tooLucidChart uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="97da3-119">When assigning a user tooLucidChart, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="97da3-120">Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="97da3-120">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toolucidchart"></a><span data-ttu-id="97da3-121">Konfigurace tooLucidChart zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="97da3-121">Configuring user provisioning tooLucidChart</span></span> 

<span data-ttu-id="97da3-122">Tato část vás provede připojením vaší služby Azure AD tooLucidChart uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v LucidChart podle přiřazení uživatelů a skupin ve službě Azure AD .</span><span class="sxs-lookup"><span data-stu-id="97da3-122">This section guides you through connecting your Azure AD tooLucidChart's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="97da3-123">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro LucidChart, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97da3-123">You may also choose tooenabled SAML-based Single Sign-On for LucidChart, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="97da3-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="97da3-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toolucidchart-in-azure-ad"></a><span data-ttu-id="97da3-125">Konfigurace automatického uživatelského účtu zřizování tooLucidChart ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="97da3-125">Configure automatic user account provisioning tooLucidChart in Azure AD</span></span>


1. <span data-ttu-id="97da3-126">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="97da3-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="97da3-127">Pokud jste již nakonfigurovali LucidChart pro jednotné přihlašování, vyhledejte instanci LucidChart pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="97da3-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using hello search field.</span></span> <span data-ttu-id="97da3-128">Jinak vyberte možnost **přidat** a vyhledejte **LucidChart** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="97da3-128">Otherwise, select **Add** and search for **LucidChart** in hello application gallery.</span></span> <span data-ttu-id="97da3-129">Vyberte LucidChart z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="97da3-129">Select LucidChart from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="97da3-130">Vyberte instanci LucidChart a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="97da3-130">Select your instance of LucidChart, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="97da3-131">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="97da3-131">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Zřizování LucidChart](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="97da3-133">V části hello **přihlašovací údaje správce** část, vstupní hello **tajný klíč tokenu** generované vaší LucidChart účet (hello token můžete najít v rámci vašeho účtu: **Team**  >  **Integrace aplikací** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="97da3-133">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your LucidChart's account (you can find hello token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![Zřizování LucidChart](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="97da3-135">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour LucidChart aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97da3-135">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour LucidChart app.</span></span> <span data-ttu-id="97da3-136">Pokud hello připojení selže, zajistěte, aby byl váš účet LucidChart oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="97da3-136">If hello connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="97da3-137">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="97da3-137">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="97da3-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="97da3-138">Click **Save**.</span></span> 

9. <span data-ttu-id="97da3-139">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooLucidChart**.</span><span class="sxs-lookup"><span data-stu-id="97da3-139">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooLucidChart**.</span></span>

10. <span data-ttu-id="97da3-140">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooLucidChart Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97da3-140">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooLucidChart.</span></span> <span data-ttu-id="97da3-141">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v LucidChart pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="97da3-141">hello attributes selected as **Matching** properties are used toomatch hello user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="97da3-142">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="97da3-142">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="97da3-143">tooenable hello zřizování služby Azure AD pro LucidChart, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="97da3-143">tooenable hello Azure AD provisioning service for LucidChart, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="97da3-144">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="97da3-144">Click **Save**.</span></span> 

<span data-ttu-id="97da3-145">Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooLucidChart v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="97da3-145">This operation starts hello initial synchronization of any users and/or groups assigned tooLucidChart in hello Users and Groups section.</span></span> <span data-ttu-id="97da3-146">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="97da3-146">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="97da3-147">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="97da3-147">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="97da3-148">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="97da3-148">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="97da3-149">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="97da3-149">Additional resources</span></span>

* [<span data-ttu-id="97da3-150">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="97da3-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="97da3-151">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97da3-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="97da3-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97da3-152">Next steps</span></span>

* [<span data-ttu-id="97da3-153">Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="97da3-153">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
