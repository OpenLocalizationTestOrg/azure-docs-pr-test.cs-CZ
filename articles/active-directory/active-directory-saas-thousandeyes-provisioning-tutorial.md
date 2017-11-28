---
title: "Kurz: Konfigurace ThousandEyes pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooThousandEyes."
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
ms.openlocfilehash: f31883ab685d0ffcd9a830aa4a7d43c056f5f4cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a><span data-ttu-id="f4a39-103">Kurz: Konfigurace ThousandEyes pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="f4a39-103">Tutorial: Configuring ThousandEyes for Automatic User Provisioning</span></span>


<span data-ttu-id="f4a39-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v zřídit tooautomatically ThousandEyes a Azure AD a zrušte zřízení uživatelských účtů z tooThousandEyes Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4a39-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in ThousandEyes and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooThousandEyes.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f4a39-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f4a39-105">Prerequisites</span></span>

<span data-ttu-id="f4a39-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f4a39-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="f4a39-107">Klienta služby Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="f4a39-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="f4a39-108">ThousandEyes klienta s hello [standardní plán](https://www.thousandeyes.com/pricing) nebo lépe povolena.</span><span class="sxs-lookup"><span data-stu-id="f4a39-108">A ThousandEyes tenant with hello [Standard plan](https://www.thousandeyes.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="f4a39-109">Uživatelský účet v ThousandEyes s oprávněními správce</span><span class="sxs-lookup"><span data-stu-id="f4a39-109">A user account in ThousandEyes with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="f4a39-110">Hello Azure AD zřizování integrace spoléhá na hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), která je k dispozici tooThousandEyes týmy hello standardní plán nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f4a39-110">hello Azure AD provisioning integration relies on hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), which is available tooThousandEyes teams on hello Standard plan or better.</span></span>

## <a name="assigning-users-toothousandeyes"></a><span data-ttu-id="f4a39-111">Přiřazení uživatelů tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="f4a39-111">Assigning users tooThousandEyes</span></span>

<span data-ttu-id="f4a39-112">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4a39-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="f4a39-113">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4a39-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="f4a39-114">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci ThousandEyes tooyour.</span><span class="sxs-lookup"><span data-stu-id="f4a39-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour ThousandEyes app.</span></span> <span data-ttu-id="f4a39-115">Jakmile se rozhodli, můžete přiřadit tyto aplikace ThousandEyes tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="f4a39-115">Once decided, you can assign these users tooyour ThousandEyes app by following hello instructions here:</span></span>

[<span data-ttu-id="f4a39-116">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="f4a39-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toothousandeyes"></a><span data-ttu-id="f4a39-117">Důležité tipy pro přiřazení uživatelů tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="f4a39-117">Important tips for assigning users tooThousandEyes</span></span>

*   <span data-ttu-id="f4a39-118">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooThousandEyes zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f4a39-118">It is recommended that a single Azure AD user is assigned tooThousandEyes tootest hello provisioning configuration.</span></span> <span data-ttu-id="f4a39-119">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="f4a39-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="f4a39-120">Při přiřazování tooThousandEyes uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello.</span><span class="sxs-lookup"><span data-stu-id="f4a39-120">When assigning a user tooThousandEyes, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="f4a39-121">Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="f4a39-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toothousandeyes"></a><span data-ttu-id="f4a39-122">Konfigurace tooThousandEyes zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="f4a39-122">Configuring user provisioning tooThousandEyes</span></span> 

<span data-ttu-id="f4a39-123">Tato část vás provede připojením vaší služby Azure AD tooThousandEyes uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v ThousandEyes podle přiřazení uživatelů a skupin v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4a39-123">This section guides you through connecting your Azure AD tooThousandEyes's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in ThousandEyes based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="f4a39-124">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro ThousandEyes, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f4a39-124">You may also choose tooenabled SAML-based Single Sign-On for ThousandEyes, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f4a39-125">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="f4a39-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toothousandeyes-in-azure-ad"></a><span data-ttu-id="f4a39-126">Konfigurace automatického uživatelského účtu zřizování tooThousandEyes ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4a39-126">Configure automatic user account provisioning tooThousandEyes in Azure AD</span></span>


1. <span data-ttu-id="f4a39-127">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="f4a39-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="f4a39-128">Pokud jste již nakonfigurovali ThousandEyes pro jednotné přihlašování, vyhledejte instanci ThousandEyes pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="f4a39-128">If you have already configured ThousandEyes for single sign-on, search for your instance of ThousandEyes using hello search field.</span></span> <span data-ttu-id="f4a39-129">Jinak vyberte možnost **přidat** a vyhledejte **ThousandEyes** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="f4a39-129">Otherwise, select **Add** and search for **ThousandEyes** in hello application gallery.</span></span> <span data-ttu-id="f4a39-130">Vyberte ThousandEyes z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="f4a39-130">Select ThousandEyes from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="f4a39-131">Vyberte instanci ThousandEyes a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="f4a39-131">Select your instance of ThousandEyes, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="f4a39-132">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="f4a39-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![ThousandEyes zřizování](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. <span data-ttu-id="f4a39-134">V části hello **přihlašovací údaje správce** část, vstupní hello **tajný klíč tokenu** generované vaší ThousandEyes účtu (hello token můžete najít v rámci účtu ThousandEyes: **zabezpečení & Ověřování**).</span><span class="sxs-lookup"><span data-stu-id="f4a39-134">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your ThousandEyes's account (you can find hello token under your ThousandEyes account: **Security & Authentication**).</span></span> 

    ![ThousandEyes zřizování](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. <span data-ttu-id="f4a39-136">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour ThousandEyes aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4a39-136">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour ThousandEyes app.</span></span> <span data-ttu-id="f4a39-137">Pokud hello připojení selže, zajistěte, aby byl váš účet ThousandEyes oprávnění správce a opakujte krok 5.</span><span class="sxs-lookup"><span data-stu-id="f4a39-137">If hello connection fails, ensure your ThousandEyes account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="f4a39-138">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."</span><span class="sxs-lookup"><span data-stu-id="f4a39-138">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="f4a39-139">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4a39-139">Click **Save**.</span></span> 

9. <span data-ttu-id="f4a39-140">V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="f4a39-140">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooThousandEyes**.</span></span>

10. <span data-ttu-id="f4a39-141">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooThousandEyes Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4a39-141">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooThousandEyes.</span></span> <span data-ttu-id="f4a39-142">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v ThousandEyes pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f4a39-142">hello attributes selected as **Matching** properties are used toomatch hello user accounts in ThousandEyes for update operations.</span></span> <span data-ttu-id="f4a39-143">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="f4a39-143">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="f4a39-144">tooenable hello zřizování služby Azure AD pro ThousandEyes, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části</span><span class="sxs-lookup"><span data-stu-id="f4a39-144">tooenable hello Azure AD provisioning service for ThousandEyes, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="f4a39-145">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4a39-145">Click **Save**.</span></span> 

<span data-ttu-id="f4a39-146">Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooThousandEyes v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="f4a39-146">This operation starts hello initial synchronization of any users and/or groups assigned tooThousandEyes in hello Users and Groups section.</span></span> <span data-ttu-id="f4a39-147">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="f4a39-147">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="f4a39-148">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.</span><span class="sxs-lookup"><span data-stu-id="f4a39-148">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="f4a39-149">Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="f4a39-149">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f4a39-150">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f4a39-150">Additional resources</span></span>

* [<span data-ttu-id="f4a39-151">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="f4a39-151">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="f4a39-152">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f4a39-152">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="f4a39-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4a39-153">Next steps</span></span>

* [<span data-ttu-id="f4a39-154">Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="f4a39-154">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
