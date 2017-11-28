---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pole."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e92baabb174642c22c99e2a30bc9c71845b3b75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a><span data-ttu-id="b862b-103">Kurz: Konfigurace pole pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="b862b-103">Tutorial: Configuring Box for Automatic User Provisioning</span></span>

<span data-ttu-id="b862b-104">cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v poli a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooBox Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b862b-104">hello objective of this tutorial is tooshow hello steps you need tooperform in Box and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooBox.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b862b-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b862b-105">Prerequisites</span></span>

<span data-ttu-id="b862b-106">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b862b-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="b862b-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="b862b-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="b862b-108">Pole jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="b862b-108">A Box single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="b862b-109">Uživatelský účet v poli s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="b862b-109">A user account in Box with Team Admin permissions.</span></span>

## <a name="assigning-users-toobox"></a><span data-ttu-id="b862b-110">Přiřazení uživatelů tooBox</span><span class="sxs-lookup"><span data-stu-id="b862b-110">Assigning users tooBox</span></span> 

<span data-ttu-id="b862b-111">Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace.</span><span class="sxs-lookup"><span data-stu-id="b862b-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="b862b-112">V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b862b-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="b862b-113">Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Box tooyour.</span><span class="sxs-lookup"><span data-stu-id="b862b-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Box app.</span></span> <span data-ttu-id="b862b-114">Jakmile se rozhodli, můžete přiřadit aplikaci Box tooyour těchto uživatelů podle pokynů hello tady:</span><span class="sxs-lookup"><span data-stu-id="b862b-114">Once decided, you can assign these users tooyour Box app by following hello instructions here:</span></span>

[<span data-ttu-id="b862b-115">Přiřadit uživatele nebo skupinu tooan firemní aplikace</span><span class="sxs-lookup"><span data-stu-id="b862b-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a><span data-ttu-id="b862b-116">Přiřazení uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="b862b-116">Assign users and groups</span></span>
<span data-ttu-id="b862b-117">Hello **pole > Uživatelé a skupiny** karta v hello portál Azure vám umožní toospecify, kteří uživatelé a skupiny udělení přístupu tooBox.</span><span class="sxs-lookup"><span data-stu-id="b862b-117">hello **Box > Users and Groups** tab in hello Azure portal allows you toospecify which users and groups should be granted access tooBox.</span></span> <span data-ttu-id="b862b-118">Přiřazení uživatele nebo skupiny, způsobí, že hello následující toooccur věcí:</span><span class="sxs-lookup"><span data-stu-id="b862b-118">Assignment of a user or group causes hello following things toooccur:</span></span>

* <span data-ttu-id="b862b-119">Azure AD umožňuje tooBox tooauthenticate hello přiřadit uživatele (buď přímé přiřazení nebo členství ve skupině).</span><span class="sxs-lookup"><span data-stu-id="b862b-119">Azure AD permits hello assigned user (either by direct assignment or group membership) tooauthenticate tooBox.</span></span> <span data-ttu-id="b862b-120">Pokud uživatel není přiřazen, Azure AD nepovoluje je toosign v tooBox a vrátí chybu na stránce přihlášení hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b862b-120">If a user is not assigned, then Azure AD does not permit them toosign in tooBox and returns an error on hello Azure AD sign-in page.</span></span>
* <span data-ttu-id="b862b-121">Dlaždici aplikace pro pole se přidá uživatele toohello [Spouštěč aplikace](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="b862b-121">An app tile for Box is added toohello user's [application launcher](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>
* <span data-ttu-id="b862b-122">Pokud je povoleno automatické zřizování, pak hello přiřazené uživatelů nebo skupin se přidají toohello zřizování fronty toobe zřizuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="b862b-122">If automatic provisioning is enabled, then hello assigned users and/or groups are added toohello provisioning queue toobe automatically provisioned.</span></span>
  
  * <span data-ttu-id="b862b-123">Pokud pouze uživatelské objekty byly nakonfigurované toobe zřízený, pak všechny přímo přiřazené uživatele jsou umístěny ve frontě zřizování hello a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny v hello zřizování fronty.</span><span class="sxs-lookup"><span data-stu-id="b862b-123">If only user objects were configured toobe provisioned, then all directly assigned users are placed in hello provisioning queue, and all users that are members of any assigned groups are placed in hello provisioning queue.</span></span> 
  * <span data-ttu-id="b862b-124">Pokud objekty skupiny byly nakonfigurované toobe zřízený, všechny objekty přiřazené skupiny jsou zřízené tooBox a všechny uživatele, kteří jsou členy těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="b862b-124">If group objects were configured toobe provisioned, then all assigned group objects are provisioned tooBox, and all users that are members of those groups.</span></span> <span data-ttu-id="b862b-125">Při zápisu tooBox zůstanou zachovány Hello skupiny a uživatele ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="b862b-125">hello group and user memberships are preserved upon being written tooBox.</span></span>

<span data-ttu-id="b862b-126">Můžete použít hello **atributy > jednotné přihlašování** kartě tooconfigure, které atributy uživatele (nebo deklarace identity) jsou uvedené tooBox během ověřování na základě SAML a hello **atributy > zřizování** kartě tooconfigure jak toku atributy uživatelů a skupin ze služby Azure AD tooBox při zřizování operace.</span><span class="sxs-lookup"><span data-stu-id="b862b-126">You can use hello **Attributes > Single Sign-On** tab tooconfigure which user attributes (or claims) are presented tooBox during SAML-based authentication, and hello **Attributes > Provisioning** tab tooconfigure how user and group attributes flow from Azure AD tooBox during provisioning operations.</span></span>

### <a name="important-tips-for-assigning-users-toobox"></a><span data-ttu-id="b862b-127">Důležité tipy pro přiřazení uživatelů tooBox</span><span class="sxs-lookup"><span data-stu-id="b862b-127">Important tips for assigning users tooBox</span></span> 

*   <span data-ttu-id="b862b-128">Dále je doporučeno jednoho Azure AD uživatel s přiřazenou tooBox tootest hello zřizování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b862b-128">It is recommended that a single Azure AD user assigned tooBox tootest hello provisioning configuration.</span></span> <span data-ttu-id="b862b-129">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="b862b-129">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="b862b-130">Při přiřazování toobox uživatele, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="b862b-130">When assigning a user toobox, you must select a valid user role.</span></span> <span data-ttu-id="b862b-131">role "Výchozí přístup" Hello nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="b862b-131">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="b862b-132">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b862b-132">Enable Automated User Provisioning</span></span>

<span data-ttu-id="b862b-133">V této části se provede připojení vaší služby Azure AD tooBox uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v poli podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b862b-133">This section guides through connecting your Azure AD tooBox's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Box based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="b862b-134">Pokud je povoleno automatické zřizování, pak hello přiřazené uživatelů nebo skupin se přidají toohello zřizování fronty toobe zřizuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="b862b-134">If automatic provisioning is enabled, then hello assigned users and/or groups are added toohello provisioning queue toobe automatically provisioned.</span></span>
    
 * <span data-ttu-id="b862b-135">Pokud pouze uživatelské objekty jsou nakonfigurované toobe zřízený, pak přímo přiřazené uživatele jsou umístěny v hello zřizování fronty, a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny v hello zřizování fronty.</span><span class="sxs-lookup"><span data-stu-id="b862b-135">If only user objects are configured toobe provisioned, then directly assigned users are placed in hello provisioning queue, and all users that are members of any assigned groups are placed in hello provisioning queue.</span></span> 
    
 * <span data-ttu-id="b862b-136">Pokud objekty skupiny byly nakonfigurované toobe zřízený, všechny objekty přiřazené skupiny jsou zřízené tooBox a všechny uživatele, kteří jsou členy těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="b862b-136">If group objects were configured toobe provisioned, then all assigned group objects are provisioned tooBox, and all users that are members of those groups.</span></span> <span data-ttu-id="b862b-137">Při zápisu tooBox zůstanou zachovány Hello skupiny a uživatele ve skupinách.</span><span class="sxs-lookup"><span data-stu-id="b862b-137">hello group and user memberships are preserved upon being written tooBox.</span></span>

> [!TIP] 
> <span data-ttu-id="b862b-138">Můžete také tooenabled na základě SAML jednotné přihlašování pro pole, hello pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b862b-138">You may also choose tooenabled SAML-based Single Sign-On for Box, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b862b-139">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="b862b-139">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="b862b-140">tooconfigure automatické uživatel účet zřizování:</span><span class="sxs-lookup"><span data-stu-id="b862b-140">tooconfigure automatic user account provisioning:</span></span>

<span data-ttu-id="b862b-141">Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooBox.</span><span class="sxs-lookup"><span data-stu-id="b862b-141">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooBox.</span></span>

1. <span data-ttu-id="b862b-142">V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="b862b-142">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="b862b-143">Pokud jste již nakonfigurovali pole pro jednotné přihlašování, vyhledejte instanci pole s použitím pole hledání hello.</span><span class="sxs-lookup"><span data-stu-id="b862b-143">If you have already configured Box for single sign-on, search for your instance of Box using hello search field.</span></span> <span data-ttu-id="b862b-144">Jinak vyberte možnost **přidat** a vyhledejte **pole** v galerii aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="b862b-144">Otherwise, select **Add** and search for **Box** in hello application gallery.</span></span> <span data-ttu-id="b862b-145">Vyberte pole ze hello výsledky vyhledávání a přidejte ji tooyour seznam aplikací.</span><span class="sxs-lookup"><span data-stu-id="b862b-145">Select Box from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="b862b-146">Vyberte instanci pole a pak vyberte hello **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="b862b-146">Select your instance of Box, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="b862b-147">Sada hello **režimu zřizování** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="b862b-147">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. <span data-ttu-id="b862b-149">V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize** tooopen přihlašovací dialogové okno pole v novém okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b862b-149">Under hello **Admin Credentials** section, click **Authorize** tooopen a Box login dialog in a new browser window.</span></span>

6. <span data-ttu-id="b862b-150">Na hello **přihlášení toogrant přístup tooBox** zadejte hello vyžaduje pověření a pak klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="b862b-150">On hello **Login toogrant access tooBox** page, provide hello required credentials, and then click **Authorize**.</span></span> 
   
    <span data-ttu-id="b862b-151">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b862b-151">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "Enable automatic user provisioning")</span></span>

7. <span data-ttu-id="b862b-152">Klikněte na tlačítko **udělit přístup tooBox** tooauthorize tuto operaci a tooreturn toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b862b-152">Click **Grant access tooBox** tooauthorize this operation and tooreturn toohello Azure portal.</span></span> 
   
    <span data-ttu-id="b862b-153">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b862b-153">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "Enable automatic user provisioning")</span></span>

8. <span data-ttu-id="b862b-154">V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour pole aplikace.</span><span class="sxs-lookup"><span data-stu-id="b862b-154">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Box app.</span></span> <span data-ttu-id="b862b-155">Pokud hello připojení nezdaří, zkontrolujte účtu pole má oprávnění správce týmu a zkuste to hello **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="b862b-155">If hello connection fails, ensure your Box account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

9. <span data-ttu-id="b862b-156">Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.</span><span class="sxs-lookup"><span data-stu-id="b862b-156">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

10. <span data-ttu-id="b862b-157">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="b862b-157">Click **Save.**</span></span>

11. <span data-ttu-id="b862b-158">V části hello části mapování, vyberte **tooBox synchronizaci uživatelů Azure Active Directory.**</span><span class="sxs-lookup"><span data-stu-id="b862b-158">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooBox.**</span></span>

12. <span data-ttu-id="b862b-159">V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooBox Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b862b-159">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooBox.</span></span> <span data-ttu-id="b862b-160">Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v poli pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b862b-160">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Box for update operations.</span></span> <span data-ttu-id="b862b-161">Vyberte toocommit tlačítko hello uložit změny.</span><span class="sxs-lookup"><span data-stu-id="b862b-161">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="b862b-162">tooenable hello zřizování služby Azure AD pro pole, změna hello **Stav zřizování** příliš**na** v části Nastavení hello</span><span class="sxs-lookup"><span data-stu-id="b862b-162">tooenable hello Azure AD provisioning service for Box, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

14. <span data-ttu-id="b862b-163">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="b862b-163">Click **Save.**</span></span>

<span data-ttu-id="b862b-164">Který spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooBox v hello uživatelé a skupiny oddílu.</span><span class="sxs-lookup"><span data-stu-id="b862b-164">That starts hello initial synchronization of any users and/or groups assigned tooBox in hello Users and Groups section.</span></span> <span data-ttu-id="b862b-165">počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello.</span><span class="sxs-lookup"><span data-stu-id="b862b-165">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="b862b-166">Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci pole.</span><span class="sxs-lookup"><span data-stu-id="b862b-166">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Box app.</span></span>

<span data-ttu-id="b862b-167">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="b862b-167">You can now create a test account.</span></span> <span data-ttu-id="b862b-168">Počkejte, až minut too20 tooverify, který hello účet byl synchronizován toobox.</span><span class="sxs-lookup"><span data-stu-id="b862b-168">Wait for up too20 minutes tooverify that hello account has been synchronized toobox.</span></span>

<span data-ttu-id="b862b-169">Ve vašem klientovi políčko synchronizovaní uživatelé jsou uvedeny v části **spravované uživatele** v hello **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="b862b-169">In your Box tenant, synchronized users are listed under **Managed Users** in hello **Admin Console**.</span></span>

<span data-ttu-id="b862b-170">![Stav integrace](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "stav integrace")</span><span class="sxs-lookup"><span data-stu-id="b862b-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "Integration status")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b862b-171">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b862b-171">Additional resources</span></span>

* [<span data-ttu-id="b862b-172">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="b862b-172">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b862b-173">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b862b-173">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b862b-174">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b862b-174">Configure Single Sign-on</span></span>](active-directory-saas-box-tutorial.md)