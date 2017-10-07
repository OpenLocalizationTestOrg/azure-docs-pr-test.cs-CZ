---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a DocuSign."
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
ms.openlocfilehash: 8562a8f9e05fb72d3331507b7da5c6afee38f9b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a><span data-ttu-id="5c321-103">Kurz: Konfigurace DocuSign pro zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="5c321-103">Tutorial: Configuring DocuSign for User Provisioning</span></span>

<span data-ttu-id="5c321-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v DocuSign a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooDocuSign Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c321-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in DocuSign and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooDocuSign.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c321-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c321-105">Prerequisites</span></span>

<span data-ttu-id="5c321-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5c321-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="5c321-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="5c321-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="5c321-108">DocuSign jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="5c321-108">A DocuSign single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="5c321-109">Uživatelský účet v DocuSign s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="5c321-109">A user account in DocuSign with Team Admin permissions.</span></span>

## <a name="assigning-users-toodocusign"></a><span data-ttu-id="5c321-110">Přiřazení uživatelů tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="5c321-110">Assigning users tooDocuSign</span></span>

<span data-ttu-id="5c321-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="5c321-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="5c321-112">V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c321-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="5c321-113">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci DocuSign tooyour.</span><span class="sxs-lookup"><span data-stu-id="5c321-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour DocuSign app.</span></span> <span data-ttu-id="5c321-114">Jakmile se rozhodli, můžete přiřadit tyto aplikace DocuSign tooyour uživatelů podle pokynů hello zde:</span><span class="sxs-lookup"><span data-stu-id="5c321-114">Once decided, you can assign these users tooyour DocuSign app by following hello instructions here:</span></span>

[<span data-ttu-id="5c321-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="5c321-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodocusign"></a><span data-ttu-id="5c321-116">Důležité tipy pro přiřazení uživatelů tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="5c321-116">Important tips for assigning users tooDocuSign</span></span>

*   <span data-ttu-id="5c321-117">Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooDocuSign zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5c321-117">It is recommended that a single Azure AD user is assigned tooDocuSign tootest hello provisioning configuration.</span></span> <span data-ttu-id="5c321-118">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="5c321-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="5c321-119">Při přiřazování tooDocuSign uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="5c321-119">When assigning a user tooDocuSign, you must select a valid user role.</span></span> <span data-ttu-id="5c321-120">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="5c321-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="5c321-121">Povolit zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="5c321-121">Enable User Provisioning</span></span>

<span data-ttu-id="5c321-122">Tato část vás provede připojením vaší služby Azure AD tooDocuSign uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v DocuSign podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c321-122">This section guides you through connecting your Azure AD tooDocuSign's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in DocuSign based on user and group assignment in Azure AD.</span></span>

> [!Tip]
> <span data-ttu-id="5c321-123">Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro DocuSign, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5c321-123">You may also choose tooenabled SAML-based Single Sign-On for DocuSign, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5c321-124">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="5c321-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="5c321-125">tooconfigure uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="5c321-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="5c321-126">Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooDocuSign účty.</span><span class="sxs-lookup"><span data-stu-id="5c321-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooDocuSign.</span></span>

1. <span data-ttu-id="5c321-127">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="5c321-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="5c321-128">Pokud jste již nakonfigurovali DocuSign pro jednotné přihlašování, vyhledejte instanci DocuSign pomocí hello vyhledávací pole.</span><span class="sxs-lookup"><span data-stu-id="5c321-128">If you have already configured DocuSign for single sign-on, search for your instance of DocuSign using hello search field.</span></span> <span data-ttu-id="5c321-129">Jinak vyberte možnost **přidat** a vyhledejte **DocuSign** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="5c321-129">Otherwise, select **Add** and search for **DocuSign** in hello application gallery.</span></span> <span data-ttu-id="5c321-130">Vyberte DocuSign z výsledků hledání hello a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="5c321-130">Select DocuSign from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="5c321-131">Vyberte instanci DocuSign a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="5c321-131">Select your instance of DocuSign, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="5c321-132">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="5c321-132">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="5c321-134">V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="5c321-134">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="5c321-135">a.</span><span class="sxs-lookup"><span data-stu-id="5c321-135">a.</span></span> <span data-ttu-id="5c321-136">V hello **uživatelské jméno správce** textovému poli, typ DocuSign účet název, který má hello **správce systému** profil v DocuSign.com přiřazen.</span><span class="sxs-lookup"><span data-stu-id="5c321-136">In hello **Admin User Name** textbox, type a DocuSign account name that has hello **System Administrator** profile in DocuSign.com assigned.</span></span>
   
    <span data-ttu-id="5c321-137">b.</span><span class="sxs-lookup"><span data-stu-id="5c321-137">b.</span></span> <span data-ttu-id="5c321-138">V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="5c321-138">In hello **Admin Password** textbox, type hello password for this account.</span></span>

6. <span data-ttu-id="5c321-139">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour DocuSign aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5c321-139">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour DocuSign app.</span></span>

7. <span data-ttu-id="5c321-140">V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="5c321-140">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox.</span></span>

8. <span data-ttu-id="5c321-141">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="5c321-141">Click **Save.**</span></span>

9. <span data-ttu-id="5c321-142">V části hello části mapování, vyberte **tooDocuSign synchronizaci uživatelů Azure Active Directory.**</span><span class="sxs-lookup"><span data-stu-id="5c321-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooDocuSign.**</span></span>

10. <span data-ttu-id="5c321-143">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooDocuSign Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c321-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooDocuSign.</span></span> <span data-ttu-id="5c321-144">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v DocuSign pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5c321-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in DocuSign for update operations.</span></span> <span data-ttu-id="5c321-145">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="5c321-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="5c321-146">tooenable hello zřizování služby Azure AD pro DocuSign, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="5c321-146">tooenable hello Azure AD provisioning service for DocuSign, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="5c321-147">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="5c321-147">Click **Save.**</span></span>

<span data-ttu-id="5c321-148">Spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooDocuSign v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="5c321-148">It starts hello initial synchronization of any users and/or groups assigned tooDocuSign in hello Users and Groups section.</span></span> <span data-ttu-id="5c321-149">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="5c321-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="5c321-150">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="5c321-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your DocuSign app.</span></span>

<span data-ttu-id="5c321-151">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="5c321-151">You can now create a test account.</span></span> <span data-ttu-id="5c321-152">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooDocuSign.</span><span class="sxs-lookup"><span data-stu-id="5c321-152">Wait for up too20 minutes tooverify that hello account has been synchronized tooDocuSign.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c321-153">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5c321-153">Additional resources</span></span>

* [<span data-ttu-id="5c321-154">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="5c321-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c321-155">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c321-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="5c321-156">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5c321-156">Configure Single Sign-on</span></span>](active-directory-saas-docusign-tutorial.md)