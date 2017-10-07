---
title: "Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooZenDesk."
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
ms.openlocfilehash: 200e8790ec1755f5cf927274ceb38527dd993f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a><span data-ttu-id="11d68-103">Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="11d68-103">Tutorial: Configuring ZenDesk for Automatic User Provisioning</span></span>


<span data-ttu-id="11d68-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Zendesku a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooZenDesk Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11d68-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in ZenDesk and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooZenDesk.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="11d68-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="11d68-105">Prerequisites</span></span>

<span data-ttu-id="11d68-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="11d68-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="11d68-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="11d68-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="11d68-108">Klientovi služby ZenDesk s hello [plánu podnikového](https://www.zendesk.com/product/pricing/) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="11d68-108">A ZenDesk tenant with hello [Enterprise plan](https://www.zendesk.com/product/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="11d68-109">Uživatelský účet v Zendesku se oprávnění správce</span><span class="sxs-lookup"><span data-stu-id="11d68-109">A user account in ZenDesk with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="11d68-110">Hello Azure AD zřizování integrace spoléhá na hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), která je k dispozici tooZenDesk týmy hello základní plán nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="11d68-110">hello Azure AD provisioning integration relies on hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), which is available tooZenDesk teams on hello Essential plan or better.</span></span>

## <a name="assigning-users-toozendesk"></a><span data-ttu-id="11d68-111">Přiřazení uživatelů tooZenDesk</span><span class="sxs-lookup"><span data-stu-id="11d68-111">Assigning users tooZenDesk</span></span>

<span data-ttu-id="11d68-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="11d68-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="11d68-113">V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11d68-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="11d68-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci služby ZenDesk tooyour.</span><span class="sxs-lookup"><span data-stu-id="11d68-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour ZenDesk app.</span></span> <span data-ttu-id="11d68-115">Jakmile se rozhodli, můžete přiřadit tyto aplikace ZenDesk tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="11d68-115">Once decided, you can assign these users tooyour ZenDesk app by following hello instructions here:</span></span>

[<span data-ttu-id="11d68-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="11d68-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toozendesk"></a><span data-ttu-id="11d68-117">Důležité tipy pro přiřazení uživatelů tooZenDesk</span><span class="sxs-lookup"><span data-stu-id="11d68-117">Important tips for assigning users tooZenDesk</span></span>

*   <span data-ttu-id="11d68-118">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooZenDesk zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="11d68-118">It is recommended that a single Azure AD user is assigned tooZenDesk tootest hello provisioning configuration.</span></span> <span data-ttu-id="11d68-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="11d68-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="11d68-120">Při přiřazování tooZenDesk uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="11d68-120">When assigning a user tooZenDesk, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="11d68-121">Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="11d68-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="11d68-122">Jako přidané funkce hello zřizování služby načte všechny vlastní role, které jsou definované v Zendesku a naimportuje je do Azure AD, kde mohou být vybrány v dialogovém okně vybrat roli hello.</span><span class="sxs-lookup"><span data-stu-id="11d68-122">As an added feature, hello provisioning service reads any custom roles defined in Zendesk, and imports them into Azure AD where they can be selected in hello Select Role dialog.</span></span> <span data-ttu-id="11d68-123">Tyto role se nebude zobrazovat v hello portál Azure po povolení hello zřizování služby a jeden synchronizační cyklus byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="11d68-123">These roles will be visible in hello Azure portal after hello provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-toozendesk"></a><span data-ttu-id="11d68-124">Konfigurace tooZenDesk zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="11d68-124">Configuring user provisioning tooZenDesk</span></span> 

<span data-ttu-id="11d68-125">Tato část vás provede připojením vaší služby Azure AD tooZenDesk uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Zendesku podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11d68-125">This section guides you through connecting your Azure AD tooZenDesk's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in ZenDesk based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="11d68-126">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro ZenDesk, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11d68-126">You may also choose tooenabled SAML-based Single Sign-On for ZenDesk, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="11d68-127">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="11d68-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toozendesk-in-azure-ad"></a><span data-ttu-id="11d68-128">Konfigurace automatického uživatelského účtu zřizování tooZenDesk ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="11d68-128">Configure automatic user account provisioning tooZenDesk in Azure AD</span></span>


1. <span data-ttu-id="11d68-129">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="11d68-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="11d68-130">Pokud jste již nakonfigurovali ZenDesk pro jednotné přihlašování, vyhledávání pro instanci služby ZenDesk pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="11d68-130">If you have already configured ZenDesk for single sign-on, search for your instance of ZenDesk using hello search field.</span></span> <span data-ttu-id="11d68-131">Jinak vyberte možnost **přidat** a vyhledejte **ZenDesk** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="11d68-131">Otherwise, select **Add** and search for **ZenDesk** in hello application gallery.</span></span> <span data-ttu-id="11d68-132">Vyberte ZenDesk z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="11d68-132">Select ZenDesk from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="11d68-133">Vyberte instanci služby ZenDesk a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="11d68-133">Select your instance of ZenDesk, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="11d68-134">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="11d68-134">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. <span data-ttu-id="11d68-136">V části hello **přihlašovací údaje správce** část, vstupní hello **uživatelské jméno správce & tokenkey & domény** generované účtu vaší ZenDesk (hello token můžete najít v rámci vašeho účtu: **správce**   >  **Rozhraní API** > **nastavení**).</span><span class="sxs-lookup"><span data-stu-id="11d68-136">Under hello **Admin Credentials** section, input hello **Admin Username&tokenkey&Domain** generated by your ZenDesk's account (you can find hello token under your account: **Admin** > **API** > **Settings**).</span></span> 

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. <span data-ttu-id="11d68-138">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour ZenDesk aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11d68-138">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour ZenDesk app.</span></span> <span data-ttu-id="11d68-139">Pokud hello připojení selže, ujistěte se, že vašeho účtu ZenDesk má oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="11d68-139">If hello connection fails, ensure your ZenDesk account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="11d68-140">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="11d68-140">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="11d68-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11d68-141">Click **Save**.</span></span> 

9. <span data-ttu-id="11d68-142">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooZenDesk**.</span><span class="sxs-lookup"><span data-stu-id="11d68-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooZenDesk**.</span></span>

10. <span data-ttu-id="11d68-143">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooZenDesk Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11d68-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooZenDesk.</span></span> <span data-ttu-id="11d68-144">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Zendesku pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="11d68-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in ZenDesk for update operations.</span></span> <span data-ttu-id="11d68-145">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="11d68-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="11d68-146">tooenable hello zřizování služby Azure AD pro ZenDesk, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="11d68-146">tooenable hello Azure AD provisioning service for ZenDesk, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="11d68-147">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11d68-147">Click **Save**.</span></span> 

<span data-ttu-id="11d68-148">Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooZenDesk v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="11d68-148">This operation starts hello initial synchronization of any users and/or groups assigned tooZenDesk in hello Users and Groups section.</span></span> <span data-ttu-id="11d68-149">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="11d68-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="11d68-150">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="11d68-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="11d68-151">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="11d68-151">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="11d68-152">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="11d68-152">Additional resources</span></span>

* [<span data-ttu-id="11d68-153">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="11d68-153">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="11d68-154">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="11d68-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="11d68-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11d68-155">Next steps</span></span>

* [<span data-ttu-id="11d68-156">Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="11d68-156">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
