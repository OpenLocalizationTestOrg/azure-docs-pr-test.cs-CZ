---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pole."
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
ms.openlocfilehash: 9f061f3f5a0a4825854b893150ceccc8951487de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a><span data-ttu-id="c9eac-103">Kurz: Konfigurace pole pro zřizování automatické uživatelů</span><span class="sxs-lookup"><span data-stu-id="c9eac-103">Tutorial: Configuring Box for Automatic User Provisioning</span></span>

<span data-ttu-id="c9eac-104">Cílem tohoto kurzu je zobrazit kroky že nutné k provedení v poli a Azure AD automaticky zřizovat a deaktivace zřízení uživatelských účtů ze služby Azure AD pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-104">The objective of this tutorial is to show the steps you need to perform in Box and Azure AD to automatically provision and de-provision user accounts from Azure AD to Box.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9eac-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9eac-105">Prerequisites</span></span>

<span data-ttu-id="c9eac-106">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="c9eac-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="c9eac-107">Klienta služby Azure Active directory.</span><span class="sxs-lookup"><span data-stu-id="c9eac-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="c9eac-108">Pole jednotného přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="c9eac-108">A Box single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="c9eac-109">Uživatelský účet v poli s oprávněními správce týmu.</span><span class="sxs-lookup"><span data-stu-id="c9eac-109">A user account in Box with Team Admin permissions.</span></span>

## <a name="assigning-users-to-box"></a><span data-ttu-id="c9eac-110">Přiřazování uživatelů do pole</span><span class="sxs-lookup"><span data-stu-id="c9eac-110">Assigning users to Box</span></span> 

<span data-ttu-id="c9eac-111">Azure Active Directory používá koncept označované jako "úlohy" k určení uživatelů, kteří obdrželi přístup k vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9eac-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="c9eac-112">V kontextu uživatele automatické zřizování účtu se synchronizují pouze uživatelé a skupiny, které byly "přiřazeny" aplikace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9eac-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="c9eac-113">Před konfigurací a povolení zřizování služby, musíte rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují uživatele, kteří potřebují přístup k vaší aplikaci pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Box app.</span></span> <span data-ttu-id="c9eac-114">Jakmile se rozhodli, můžete přiřadit tyto uživatele do pole aplikace podle pokynů tady:</span><span class="sxs-lookup"><span data-stu-id="c9eac-114">Once decided, you can assign these users to your Box app by following the instructions here:</span></span>

[<span data-ttu-id="c9eac-115">Přiřazení uživatele nebo skupiny do aplikace enterprise</span><span class="sxs-lookup"><span data-stu-id="c9eac-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a><span data-ttu-id="c9eac-116">Přiřazení uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="c9eac-116">Assign users and groups</span></span>
<span data-ttu-id="c9eac-117">**Pole > Uživatelé a skupiny** na portálu Azure umožňuje určit, kteří uživatelé a skupiny musí mít udělen přístup k poli.</span><span class="sxs-lookup"><span data-stu-id="c9eac-117">The **Box > Users and Groups** tab in the Azure portal allows you to specify which users and groups should be granted access to Box.</span></span> <span data-ttu-id="c9eac-118">Přiřazení uživatele nebo skupiny, způsobí, že následující kroky, kterými dojít:</span><span class="sxs-lookup"><span data-stu-id="c9eac-118">Assignment of a user or group causes the following things to occur:</span></span>

* <span data-ttu-id="c9eac-119">Azure AD umožňuje přiřazený uživatel (buď přímé přiřazení nebo členství ve skupině) k ověření pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-119">Azure AD permits the assigned user (either by direct assignment or group membership) to authenticate to Box.</span></span> <span data-ttu-id="c9eac-120">Pokud uživatel není přiřazen, Azure AD je pro přihlášení k pole nepovoluje a vrátí chybu na stránce pro přihlášení k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9eac-120">If a user is not assigned, then Azure AD does not permit them to sign in to Box and returns an error on the Azure AD sign-in page.</span></span>
* <span data-ttu-id="c9eac-121">Dlaždici aplikace pro pole se přidá do uživatele [Spouštěč aplikace](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="c9eac-121">An app tile for Box is added to the user's [application launcher](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>
* <span data-ttu-id="c9eac-122">Pokud je povoleno automatické zřizování, pak přiřazené uživatele nebo skupin se přidají do zřizování fronty automaticky zřídit.</span><span class="sxs-lookup"><span data-stu-id="c9eac-122">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
  
  * <span data-ttu-id="c9eac-123">Pokud pouze uživatelské objekty byly nakonfigurovány zřídit, pak všechny přímo přiřazené uživatele jsou umístěny ve frontě zřizování a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny ve frontě zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9eac-123">If only user objects were configured to be provisioned, then all directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
  * <span data-ttu-id="c9eac-124">Pokud objekty skupiny byly nakonfigurovány zřídit, jsou všechny objekty přiřazené skupiny zřízené pole a všechny uživatele, kteří jsou členy těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="c9eac-124">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="c9eac-125">Členství ve skupině a uživateli se zachovají při zápisu do pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-125">The group and user memberships are preserved upon being written to Box.</span></span>

<span data-ttu-id="c9eac-126">Můžete použít **atributy > jednotné přihlašování** karta konfigurace, které atributy uživatele (nebo deklarace identity) jsou uvedené pole během ověřování na základě SAML a **atributy > zřizování** kartu a nakonfigurovat způsob, jakým se atributy uživatelů a skupin probíhá z Azure AD pole během zřizování operace.</span><span class="sxs-lookup"><span data-stu-id="c9eac-126">You can use the **Attributes > Single Sign-On** tab to configure which user attributes (or claims) are presented to Box during SAML-based authentication, and the **Attributes > Provisioning** tab to configure how user and group attributes flow from Azure AD to Box during provisioning operations.</span></span>

### <a name="important-tips-for-assigning-users-to-box"></a><span data-ttu-id="c9eac-127">Důležité tipy pro přiřazování uživatelů do pole</span><span class="sxs-lookup"><span data-stu-id="c9eac-127">Important tips for assigning users to Box</span></span> 

*   <span data-ttu-id="c9eac-128">Dále je doporučeno jednoho přiřazeného pole testování zřizování konfigurace uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9eac-128">It is recommended that a single Azure AD user assigned to Box to test the provisioning configuration.</span></span> <span data-ttu-id="c9eac-129">Další uživatele nebo skupiny může být přiřazen později.</span><span class="sxs-lookup"><span data-stu-id="c9eac-129">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="c9eac-130">Při přiřazování uživatele pole, musíte vybrat platné uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="c9eac-130">When assigning a user to box, you must select a valid user role.</span></span> <span data-ttu-id="c9eac-131">Roli "Výchozí přístup" nefunguje pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9eac-131">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="c9eac-132">Povolit automatické zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="c9eac-132">Enable Automated User Provisioning</span></span>

<span data-ttu-id="c9eac-133">Tato část provede připojení k pole uživatelský účet zřizování rozhraní API služby Azure AD a konfiguraci zřizování službu, kterou chcete vytvořit, aktualizovat a zakažte přiřazené uživatelské účty v poli podle přiřazení uživatelů a skupin ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9eac-133">This section guides through connecting your Azure AD to Box's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Box based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="c9eac-134">Pokud je povoleno automatické zřizování, pak přiřazené uživatele nebo skupin se přidají do zřizování fronty automaticky zřídit.</span><span class="sxs-lookup"><span data-stu-id="c9eac-134">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
    
 * <span data-ttu-id="c9eac-135">Pokud pouze uživatelské objekty jsou nakonfigurované zřídit, pak přímo přiřazené uživatele jsou umístěny v zřizování fronty, a všechny uživatele, kteří jsou členy jakékoli přiřazených skupin jsou umístěny ve frontě zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9eac-135">If only user objects are configured to be provisioned, then directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
    
 * <span data-ttu-id="c9eac-136">Pokud objekty skupiny byly nakonfigurovány zřídit, jsou všechny objekty přiřazené skupiny zřízené pole a všechny uživatele, kteří jsou členy těchto skupin.</span><span class="sxs-lookup"><span data-stu-id="c9eac-136">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="c9eac-137">Členství ve skupině a uživateli se zachovají při zápisu do pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-137">The group and user memberships are preserved upon being written to Box.</span></span>

> [!TIP] 
> <span data-ttu-id="c9eac-138">Můžete také povolit na základě SAML jednotné přihlašování pro pole, postupujte podle pokynů uvedených v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c9eac-138">You may also choose to enabled SAML-based Single Sign-On for Box, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c9eac-139">Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.</span><span class="sxs-lookup"><span data-stu-id="c9eac-139">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="c9eac-140">Konfigurace automatického uživatele zřizování účtu:</span><span class="sxs-lookup"><span data-stu-id="c9eac-140">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="c9eac-141">Cílem této části se popisují postup povolení zřizování uživatelských účtů služby Active Directory do pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-141">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Box.</span></span>

1. <span data-ttu-id="c9eac-142">V [portál Azure](https://portal.azure.com), vyhledejte **Azure Active Directory > podnikové aplikace > všechny aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="c9eac-142">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="c9eac-143">Pokud jste již nakonfigurovali pole pro jednotné přihlašování, vyhledejte instanci pole s použitím pole hledání.</span><span class="sxs-lookup"><span data-stu-id="c9eac-143">If you have already configured Box for single sign-on, search for your instance of Box using the search field.</span></span> <span data-ttu-id="c9eac-144">Jinak vyberte možnost **přidat** a vyhledejte **pole** v galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9eac-144">Otherwise, select **Add** and search for **Box** in the application gallery.</span></span> <span data-ttu-id="c9eac-145">Vyberte pole ve výsledcích hledání a přidejte ji do seznamu aplikací.</span><span class="sxs-lookup"><span data-stu-id="c9eac-145">Select Box from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="c9eac-146">Vyberte instanci pole a pak vyberte **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="c9eac-146">Select your instance of Box, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="c9eac-147">Nastavte **režimu zřizování** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="c9eac-147">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Zřizování](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. <span data-ttu-id="c9eac-149">V části **přihlašovací údaje správce** klikněte na tlačítko **Autorizovat** otevřete dialogové okno přihlášení pole v novém okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c9eac-149">Under the **Admin Credentials** section, click **Authorize** to open a Box login dialog in a new browser window.</span></span>

6. <span data-ttu-id="c9eac-150">Na **přihlášení k udělení přístupu k poli** zadejte požadované pověření a pak klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="c9eac-150">On the **Login to grant access to Box** page, provide the required credentials, and then click **Authorize**.</span></span> 
   
    <span data-ttu-id="c9eac-151">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c9eac-151">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "Enable automatic user provisioning")</span></span>

7. <span data-ttu-id="c9eac-152">Klikněte na tlačítko **udělit přístup k poli** autorizovat tuto operaci a vrátit na portál Azure.</span><span class="sxs-lookup"><span data-stu-id="c9eac-152">Click **Grant access to Box** to authorize this operation and to return to the Azure portal.</span></span> 
   
    <span data-ttu-id="c9eac-153">![Povolit automatické uživatele zajišťování](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "povolit zřizování automatické uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c9eac-153">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "Enable automatic user provisioning")</span></span>

8. <span data-ttu-id="c9eac-154">Na portálu Azure klikněte na tlačítko **Test připojení** zajistit Azure AD může připojit k vaší aplikaci Box.</span><span class="sxs-lookup"><span data-stu-id="c9eac-154">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Box app.</span></span> <span data-ttu-id="c9eac-155">Pokud připojení nezdaří, zkontrolujte účtu pole má oprávnění správce týmu a zkuste to **"Ověřit"** krok opakujte.</span><span class="sxs-lookup"><span data-stu-id="c9eac-155">If the connection fails, ensure your Box account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="c9eac-156">Zadejte e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v **e-mailové oznámení** pole a zaškrtnutím políčka.</span><span class="sxs-lookup"><span data-stu-id="c9eac-156">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="c9eac-157">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="c9eac-157">Click **Save.**</span></span>

11. <span data-ttu-id="c9eac-158">V části mapování vyberte **synchronizaci Azure Active Directory Users pole.**</span><span class="sxs-lookup"><span data-stu-id="c9eac-158">Under the Mappings section, select **Synchronize Azure Active Directory Users to Box.**</span></span>

12. <span data-ttu-id="c9eac-159">V **mapování atributů** , projděte si uživatelské atributy, které jsou synchronizované z Azure AD pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-159">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Box.</span></span> <span data-ttu-id="c9eac-160">Atributy vybrán jako **párování** vlastnosti se používají tak, aby odpovídaly uživatelské účty v poli pro operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c9eac-160">The attributes selected as **Matching** properties are used to match the user accounts in Box for update operations.</span></span> <span data-ttu-id="c9eac-161">Kliknutím na tlačítko Uložit potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="c9eac-161">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="c9eac-162">Povolit Azure AD zřizování služby pro pole, změňte **Stav zřizování** k **na** v části Nastavení</span><span class="sxs-lookup"><span data-stu-id="c9eac-162">To enable the Azure AD provisioning service for Box, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="c9eac-163">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="c9eac-163">Click **Save.**</span></span>

<span data-ttu-id="c9eac-164">Která spustí počáteční synchronizaci všech uživatelů a skupiny přiřazené pole v části Uživatelé a skupiny.</span><span class="sxs-lookup"><span data-stu-id="c9eac-164">That starts the initial synchronization of any users and/or groups assigned to Box in the Users and Groups section.</span></span> <span data-ttu-id="c9eac-165">Počáteční synchronizace trvá déle než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou provést.</span><span class="sxs-lookup"><span data-stu-id="c9eac-165">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="c9eac-166">Můžete použít **podrobnosti synchronizace** části monitorovat průběh a odkazech zřízení sestavy aktivity, které popisují všechny akce, které provádí službu zřizování na pole aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9eac-166">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Box app.</span></span>

<span data-ttu-id="c9eac-167">Nyní můžete vytvořit testovací účet.</span><span class="sxs-lookup"><span data-stu-id="c9eac-167">You can now create a test account.</span></span> <span data-ttu-id="c9eac-168">Chcete-li ověřit, že účet byl synchronizován čekat až 20 minut pole.</span><span class="sxs-lookup"><span data-stu-id="c9eac-168">Wait for up to 20 minutes to verify that the account has been synchronized to box.</span></span>

<span data-ttu-id="c9eac-169">Ve vašem klientovi políčko synchronizovaní uživatelé jsou uvedeny v části **spravované uživatele** v **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="c9eac-169">In your Box tenant, synchronized users are listed under **Managed Users** in the **Admin Console**.</span></span>

<span data-ttu-id="c9eac-170">![Stav integrace](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "stav integrace")</span><span class="sxs-lookup"><span data-stu-id="c9eac-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "Integration status")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c9eac-171">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c9eac-171">Additional resources</span></span>

* [<span data-ttu-id="c9eac-172">Správa uživatelů zřizování účtu pro podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="c9eac-172">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9eac-173">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9eac-173">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c9eac-174">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9eac-174">Configure Single Sign-on</span></span>](active-directory-saas-box-tutorial.md)